meta-id: 00b43a606c790c79ebaa2753eeb9e329005d3f4f

meta-tags: Haxe
meta-publishedOn: 2015-12-01

The [API docs for sys.FileStat](http://api.haxe.org/sys/FileStat.html#ctime) state that `ctime` returns `the creation time for the file (not all filesystems support this)`.

This returns correctly on Windows, but not on Linux (I tested with both `cpp` and `neko` targets).

Windows stores the file creation time. On Linux, the story is more complicated. `ctime` is a C++ API. Windows stores the file creation time.  On Linux, according to [this Unix SE question](http://unix.stackexchange.com/a/20464/64805) (which is mostly derived from `man` pages):

> Most unices do not have a concept of file creation time. [...]
> Note that **the ctime** (`ls -lc`) **is not the file creation time**, it's the [inode](http://en.wikipedia.org/wiki/Inode) change time. The inode change time is updated whenever anything about the file changes (contents or metadata) except that the ctime isn't updated when the file is merely read (even if the atime is updated). In particular, the ctime is always more recent than the mtime (file content modification time) unless the mtime has been explicitly set to a date in the future.

I couldn't trace the Haxe source all the way down the stack, but I believe it does access the Unix `ctime` on Unix. This leaves us in a bind: how do we know the file creation time on Unix?

One option I came up with is to use [sys.io.Process](http://api.haxe.org/sys/io/Process.html) to fork a process, execute a command like `stat` to get the file time, and parse the output. This is possible, but on Ubuntu 14.04, I tested this, and the file birth time is unknown (the filesystem doesn't store it). This is also expensive, and the results should be cached.

If this is not possible or performant, the other option may be to store the time somewhere yourself. I ran into this while creating [Butterfly](http://github.com/ashes999/butterfly), a static blog generator which uses file time to guess the blog post publish date. Jekyll (a similar, Ruby based appliaction) requires users enter the post time in the filename.

There may be other solutions, but I don't know of any.
