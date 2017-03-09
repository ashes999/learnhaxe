meta-id: 2021132733bbb4db085755a1bcac908c5a3f4ed7
meta-title: Drawing Images Transparently in Kha
meta-tags: Kha
meta-publishedOn: 2017-03-08

In Kha, you can draw images semi-transparently. This doesn't refer to drawing parts of the image transparently, but rather drawing the image as a partially-transparent "layer" (eg. 25% transparent).

Although you might expect to find an `alpha` or `opacity` field on the `Image` class. Kha keeps the `opacity` field on the `Graphics` class. For example, `kha.graphics.Graphics2` has an `opacity` field. You can use it like so: 

```
public function render(g:Graphics): Void
{
	g.begin(0); // black
	g.drawImage(mountains, 0, 0); // drawn fully opaque
	g.opacity = 0.25;
	g.drawImage(ball, 100, 50); // draws the ball at 25% transparency
	g.end();
}
```

Be careful to reset `opacity` back to `1.0` when you're done, so that anything drawn after the transparent image appears normal.