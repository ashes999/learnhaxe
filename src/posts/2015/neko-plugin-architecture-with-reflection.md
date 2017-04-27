meta-id: 0724a812e5fbff39df8050b33513d0f73af5429b

meta-tags: Haxe
meta-publishedOn: 2015-02-18

The plugin architecture allows you to create apps or games that can tolerate plugins: custom code that others (or you) wrote that you load and execute at runtime.

In languages like C# and Java, you can do this by:

- Creating your main application
- Creating an interface for plugins (either in the main app or a separate project)
- Loading compiled code at runtime
- Looking for classes that implement your interfaces
- Instantiating and calling those classes reflectively

Haxe also supports this functionality, but it took some research, and works with some caveats. Let's begin.

# Main Application

My main application will be a console app with a simple `Speaks` interface. When run, the application finds all classes that implement `Speaks`, instantiate them, and call the `speak` method.

The interface looks like this:

```
public interface Speak {
  function speak() : String;
}
```

To test this, I create a simple `Cat` class:

```
// Cat.hx
class Cat implements Speak {
  public function speak() : String {
    return "Meow!";
  }
}

// Main.hx
class Main {
  public static function main() : Void {
    trace(new Cat().speak());
  }
}
```

Simple. One important key to all this is that **I build and execute my code against neko**, like so: `haxe -main Main -neko main.n`

I then run `neko main.n` which prints `Meow!`

# Creating a Dog Plugin

I create a new project with a `Dog` class that implements `Speak`:

```
// Dog.hx
class Dog implements Speak {
  public function speak() : String {
    return "WOOF!";
  }
}
```

To compile it, I reference my initial code repository, which contains my interface class source. (Alternatively, I could have both the main app and the plugin reference a separate, interface-only project.)

To compile:

`haxe Dog -neko plugin.n -cp ../main_app`

(My main application lives in the `main_app` directory, and my plugin in the `plugin` directory; both have a common root directory.)

The code creates `plugin.n`.

# Linking it Together: Finding Dog at Runtime

First, I copied `plugin.n` into `main_app\plugins`.

Now, the secret sauce:
- Load the module at runtime
- Iterate over all classes
- Iterate over all interfaces
- Look for the `Speak` interface
- If it exists, instantiate the class, and call `speak`

The code looks like this:

```
  import neko.vm.Loader;
  import neko.vm.Module;

  function makeAllSpeakersSpeak() : Void
  {
    var loader = Loader.local();
    // Loads the plugin class
    var module = loader.loadModule("plugins/plugin");
    var classes:Dynamic = module.exportsTable().__classes;

    // Dynamic object; fields are classes (Class instances)
    var classesTypes = Reflect.fields(classes);
    for (i in classesTypes)
    {
      var c:Dynamic = Reflect.field(classes, i);
      if (isSpeakClass(c))
      {
        createInstanceAndSpeak(c);
      }
    }
  }

  function isSpeakClass(c:Dynamic) : Bool
  {
    var interfaces:Dynamic = c.__interfaces__;
    var interfaceTypes = Reflect.fields(interfaces);
    // Dynamic class: array of interface Class instances
    for (interfaceType in interfaceTypes)
    {
      var interfaceArray:Dynamic = Reflect.field(interfaces, interfaceType);
      for (i in 0 ... interfaces.length) {
        var inter = interfaces[i];
        // Looks deceptive, but interfaces only have one name. It's an array ...
        if (inter.__name__[0] == 'Speak') {
          return true;
        }
      }
    }
    return false;
  }

  function createInstanceAndSpeak(c:Dynamic) : Void
  {
    // Requires a constructor with no parameters
    var instance = Type.createInstance(c, []);
    trace(instance.speak());
  }
```

That's it! The code correctly prints out `Meow!` and `WOOF!`. We have a trivial, albeit working example, of loading and executing plugin code at runtime.

# Caveats and Extensions

To recap and summarize, the caveats include:

- Only works in Neko. (You can't access neko namespaces in any other platform.)
- Doesn't work with haxe running with `--interp`; that results in a runtime error: `File "interp.ml", line 3407, characters 43-49: Assertion failed`
- Classes that implement the interface must all have the same constructor (in this case, a no-parameter constructor)
- Your client/consumer classes must have access to the original source of the interface class
- The code contains a hard-coded reference to the interface class name (`Speak`).
- The interfaces are on the plugin side; the classes are dynamic, so invocation fails at runtime if your interface changes.

To extend and improve this example, we could:

- Add some parameters to pass in to our `speak` method to allow the plugins to execute useful code that interfaces with our main application (such as passing in an object to manipulate).
- Add multiple plugin modules, and load them all at runtime, instead of just one
- Consume the interface class from a binary/executable (how?)
