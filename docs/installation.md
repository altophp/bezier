# Installation

Alto Bezier requires PHP 8.3 or newer. Install it with Composer:

```bash
composer require alto/bezier
```

## Verify the installation

Create a quadratic curve and evaluate its midpoint:

```php
<?php

require __DIR__.'/vendor/autoload.php';

use Alto\Bezier\Point;
use Alto\Bezier\QuadraticCurve;

$curve = new QuadraticCurve(
    new Point(0, 0),
    new Point(50, 100),
    new Point(100, 0),
);

echo $curve->pointAt(0.5).PHP_EOL;
```

Run the script from the project root. It prints:

```text
(50, 50)
```

Continue with [Getting started](getting-started.md) to evaluate, measure, and
split a curve.
