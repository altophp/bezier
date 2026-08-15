# Curve types

Choose a curve by the number of control points required by the source data or
the shape you need to preserve.

| Curve | Degree | Points | Use it for |
| --- | ---: | ---: | --- |
| [Quadratic](quadratic.md) | 2 | 3 | Simple arcs with one control handle; SVG `Q` segments |
| [Cubic](cubic.md) | 3 | 4 | Independent start and end handles; SVG `C` segments |
| [Quartic](quartic.md) | 4 | 5 | Existing degree-four data that must remain exact |
| [Quintic](quintic.md) | 5 | 6 | Existing degree-five data that must remain exact |
| [N-order](n-order.md) | Any degree from 1 | 2 or more | A control-point count determined at runtime |

All curve classes extend `Curve`. They share evaluation, sampling, length,
distance parameterization, tangents, normals, splitting, bounding boxes, and
intersection detection.

## Points

`Point` is an immutable pair of floating-point coordinates:

```php
use Alto\Bezier\Point;

$point = new Point(10.5, -2.3);
$samePoint = Point::fromArray([10.5, -2.3]);
$distance = $point->getDistance(new Point(0, 0));
```

Every curve can export its control points as coordinate pairs with `toArray()`.
See [Operations](../operations.md) for the shared curve contract.
