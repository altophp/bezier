# N-order curves

`NOrderCurve` accepts any number of control points from two upward. Use it when
the degree is determined at runtime or when importing curve data that does not
fit one of the dedicated classes.

<img src="../assets/norder-curve.svg" alt="An arbitrary-degree Bézier curve" width="240" height="200" />

## Create a curve

Use `Curve::fromPoints()` when coordinates already arrive as arrays:

```php
<?php

require __DIR__.'/vendor/autoload.php';

use Alto\Bezier\Curve;

$curve = Curve::fromPoints([
    [10, 150],
    [30, 20],
    [70, 180],
    [120, 10],
    [170, 190],
    [210, 40],
    [230, 150],
]);

echo 'Degree: '.$curve->degree.PHP_EOL;
echo 'Midpoint: '.$curve->pointAt(0.5).PHP_EOL;
```

The result is:

```text
Degree: 6
Midpoint: (120, 100.15625)
```

Construct `NOrderCurve` directly when the inputs are already `Point` objects.
Its `degree` property is always the control-point count minus one.

Use a dedicated class when the degree is known and its direct SVG mapping or
named control-point properties are useful.
