meta-id: f9b47ee908e9ebabce9fabbfdd7596e3cd65dd27

meta-tags: Luxe
meta-publishedOn: 2016-07-29

Disclaimer: I just started using Luxe. First impression: engine seems very usable (great API design) and things work well. But, very poorly documented (API docs are sparse and there aren't many blog posts about how to do things using Luxe.)

I'm working on a [small arena 2D shooter in Luxe](https://github.com/ashes999/opal-quest/tree/luxe). I have a ship (`Sprite` instance) and a bunch of walls (also `Sprite` instances). How can I:

- Find out when the ship hits a wall?
- Prevent the ship from moving into the wall, or move the ship back out of the wall after the collision?

Luxe includes a number of packages/classes related to collision detection. I stumbled upon the [`Polygon` class](http://luxeengine.com/docs/api/luxe/collision/shapes/Polygon.html). It includes two helpful methods:

- `Polygon.rectangle(x, y, width, height):` create a rectangular polygon, specifying position and size.
- `polygonInstance.testPolygon(polygon):` check if `polygonInstance` intersects `testPolygon`.

if `testPolygon` finds an intersection, it returns a non-null object with a number of fields. Of interest to us is the `separation` property, which tells us how much to move `polygonInstance` to get it to stop colliding with `polygon`.

Putting these together, we get something like this:

```
var shipPolygon = Polygon.rectangle(ship.pos.x, ship.pos.y, ship.size.x, ship.size.y);
var wallPolygon = Polygon.rectangle(wall.pos.x, wall.pos.y, wall.size.x, wall.size.y);
var result = shipPolygon.testPolygon(wall);
if (result != null)
{
    ship.pos.x += result.separation.x;
    ship.pos.y += result.separation.y;
}
```

This correctly causes the `ship` to stop when it hits `wall`.
