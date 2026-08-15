# Quadratic curves

A quadratic Bézier curve has a start point, one control point, and an end
point. Use it for a simple arc controlled by one handle or when preserving an
SVG `Q` segment.

<img src="../assets/quadratic-curve.svg" alt="A quadratic Bézier curve" width="240" height="200" />

## Create a curve

`QuadraticCurve` accepts its three points in path order:

```php
<?php

require __DIR__.'/vendor/autoload.php';

use Alto\Bezier\Point;
use Alto\Bezier\QuadraticCurve;

$curve = new QuadraticCurve(
    new Point(10, 170),
    new Point(140, 20),
    new Point(230, 170),
);

echo $curve->pointAt(0.5).PHP_EOL;
```

The midpoint is:

```text
(130, 95)
```

The public properties `p0`, `p1`, and `p2` retain the original points. They map
directly to `M 10 170 Q 140 20 230 170` in an SVG path.

The [shared operations](../operations.md) cover splitting, length, orientation,
and intersections.
