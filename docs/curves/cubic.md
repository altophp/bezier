# Cubic curves

A cubic Bézier curve has a start point, two control points, and an end point.
Its two handles control the departure and arrival directions independently. It
also maps directly to an SVG `C` segment.

<img src="../assets/cubic-curve.svg" alt="A cubic Bézier curve" width="240" height="200" />

## Create a curve

`CubicCurve` accepts its four points in path order:

```php
<?php

require __DIR__.'/vendor/autoload.php';

use Alto\Bezier\CubicCurve;
use Alto\Bezier\Point;

$curve = new CubicCurve(
    new Point(10, 160),
    new Point(60, 20),
    new Point(180, 20),
    new Point(230, 160),
);

echo $curve->pointAt(0.5).PHP_EOL;
```

The midpoint is:

```text
(120, 55)
```

The public properties `p0` through `p3` retain the original points. They map to
`M 10 160 C 60 20 180 20 230 160` in an SVG path.

See [Recipes](../recipes.md) for converting SVG coordinates and producing
constant-speed motion.
