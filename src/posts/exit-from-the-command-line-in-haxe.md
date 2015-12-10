meta-id: ddc5bfb3fade2c512c26aebca7574269632dcb20

meta-tags: Haxe
meta-publishedOn: 2015-11-27

To exit from the command-line in Haxe, you can use `Sys.exit(n)`, where `n` is the exit code. (`0` indicates normal termination, and anything non-zero is usually treated as an error.)

Note that this is not `sys.exit(n)` (with a lower-case S -- the `sys` package), but `Sys`, [the class](http://api.haxe.org/Sys.html) that provides access to system APIs.
