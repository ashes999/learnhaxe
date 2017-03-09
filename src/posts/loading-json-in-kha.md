meta-id: aa4f65be081cd424ded6e32161b73ad89334316e
meta-title: Loading JSON in Kha
meta-tags: Kha
meta-publishedOn: 2016-09-30

With some macro magic, Kha allows you to load assets asynchronously. Here's how that looks with images:

```
Assets.loadEverything(function()
{
  var playerImage = Assets.images.player;
}
```

If you have a `player.png` file in `Assets/images`, this snippet loads that data into the `playerImage` variable.

That works great with images, but what about JSON? Can I add `monsters.json` to `Assets/data` and load it with `Assets.images.data.monsters`?

Nope. This doesn't compile; if you check the [Assets class documentation](http://api.kha.technology/kha/Assets.html), you'll notice that it has `blobs`, `fonts`, `images`, `sounds`, `videos`. Of these, `blobs` looks promising, but the documentation for `BlobList` is non-existent.

[Some of the wiki documentation](https://github.com/KTXSoftware/Kha/wiki/Managing-Your-Assets) mentions that:

> Any other format [other than images, sounds, and vidos] will not be changed and will be available in your code through `Assets.blobs` as `Bytes`.

It turns out that you can load your data using `Assets.blobs.monsters_json`. This is a little unintuitive (no directory structure/naming?), but it works. It also gives you back `Bytes`; you can call `.toString()` on it to read it as text.

If we put this JSON into `Assets/data/monsters.json`:

```
{
    "names": [
        "slime", "bat", "spider"
    ]
}
```

We can use this code to get back `names` as an `Array<String>`:

```
Assets.loadEverything(function()
{
    var monsterNames = Json.parse(Assets.blobs.monsters_json.toString()).names;
}
```

Try to keep your filenames distinct, even across directories, to avoid naming collisions.
