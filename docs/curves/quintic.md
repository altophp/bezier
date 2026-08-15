# Quintic curves

A quintic Bézier curve has six control points and degree five. Use it when the
source data contains a degree-five curve that must remain exact rather than be
reduced to lower-degree segments.

<img src="../assets/quintic-curve.svg" alt="A quintic Bézier curve" width="240" height="200" />

## Create a curve

```php
<?php

require __DIR__.'/vendor/autoload.php';

use Alto\Bezier\Point;
use Alto\Bezier\QuinticCurve;

$curve = new QuinticCurve(
    new Point(10, 165),
    new Point(50, 40),
    new Point(90, 10),
    new Point(150, 190),
    new Point(200, 20),
    new Point(230, 160),
);

echo $curve->pointAt(0.5).PHP_EOL;
```

The midpoint is:

```text
(121.5625, 82.03125)
```

The public properties `p0` through `p5` retain the original points. SVG has no
native quintic path command, so use sampling or a conversion suited to the
target renderer.

The [shared operations](../operations.md) apply without quintic-specific
methods.
