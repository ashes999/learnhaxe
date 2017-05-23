meta-id: 43f1eb6216f4783f764eed95f81db3af47f439b1

meta-title: Making PUT Calls in the Javascript Target
meta-tags: Haxe
meta-publishedOn: 2017-05-23

How can you write a Haxe client that can make PUT calls?

According to [this StackOverflow question](https://stackoverflow.com/questions/27345757/how-to-send-http-put-request-from-haxe-neko), you can use [haxe.Http](http://api.haxe.org/haxe/Http.html) to make web requests. You can even use the `customRequest` method to make `PUT`/`PATCH`/etc. But, as of writing (with Haxe 3.4.0), this method doesn't work on the Javascript target.

After asking on the mailing list, Franco Ponticelli pointed me to his lovely `thx` library. I successfully made a REST `GET`, `POST`, and `PUT` call on Neko and JS with his library.  Here's how I did it:

- Install (`thx.http`)[https://github.com/fponticelli/thx.http] from GitHub. That is: clone the repository, and run `haxelib dev thx.http .` from the repository directory.
- Create a new Haxe project and create a shell/batch file to compile it, including the `thx.http` library as a dependency. (Sample: `haxe -main webclient.Main -cp webclient -js WebClient.js -lib thx.http`)
- Create the actual method, accepting (or hard-coding) input parameters (URL, HTTP metohd, and body).

Here's my sample method from prototyping:

```
public static function httpRequest(method:String, url:String):Void
{
    var body:String = "";

    if (method != "GET") {
        body = "{title: 'foo', body: 'bar', userId: 1}";
    }

    var info:RequestInfo;

    if (method == "GET") {
        info = new RequestInfo(Get, url, [
            "Agent" => "thx.http.Request"
        ]);
    } else {
        info = new RequestInfo(Put, url, Text(body));
    }

    Request.make(info, Json)
    .response
    .flatMap(function(r) {
        trace('${method} DONE (r=${r.statusCode}): ${r.body}');
        return r.body;
    })
    .success(function(r) trace("Request successful: " + r))
    .failure(function(e) trace("Request FAILED: " + e));
}
``` 

Notice it uses promises (via the `thx` libraries) to make the HTTP request and print out the results. 

I compiled this against Neko and JS and both targets run and make a POST call successfully.

For testing, you can use the [JSON Placeholder](https://jsonplaceholder.typicode.com/) faux web-service. It allows you to make `GET`/`POST`/`PUT`/etc. calls, with minimal validation.