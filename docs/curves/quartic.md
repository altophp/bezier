# Quartic curves

A quartic Bézier curve has five control points and degree four. Use it when the
source data already has five points and reducing it to a cubic curve would
change the path.

<img src="../assets/quartic-curve.svg" alt="A quartic Bézier curve" width="240" height="200" />

## Create a curve

```php
<?php

require __DIR__.'/vendor/autoload.php';

use Alto\Bezier\Point;
use Alto\Bezier\QuarticCurve;

$curve = new QuarticCurve(
    new Point(10, 170),
    new Point(60, 10),
    new Point(120, 190),
    new Point(180, 20),
    new Point(230, 170),
);

echo $curve->pointAt(0.5).PHP_EOL;
```

The midpoint is:

```text
(120, 100)
```

The public properties `p0` through `p4` retain the original points. SVG has no
native quartic path command, so renderers must sample the curve or convert it
to supported segments.

See [Operations](../operations.md) for sampling the curve with `points()`.
