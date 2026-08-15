# Alto Bezier

Alto\Bezier is a precision PHP toolkit for evaluating, splitting, and measuring quadratic, cubic, quintic, and
N-order Bézier curves using immutable value objects and a numerically stable De Casteljau core.

## Features

- Quadratic, cubic, quartic, quintic, and arbitrary N-order curves that all share the same API
- Point evaluation: `pointAt(t)` and constant-speed `pointAtDistance(distance)` helpers
- Curve subdivision with `split(t)` for tessellation or editing workflows
- Differential geometry helpers: derivatives, tangents, normals
- Arc-length calculation and arc-length parameterization with configurable sampling
- Intersection detection between any two curves
- Bounding boxes and evenly spaced point generators for export to SVG, canvas, or WebGL
- Immutable, readonly classes with no external dependencies

## Installation

```bash
composer require alto/bezier
```

## Usage

### Create and inspect a cubic curve

```php
use Alto\Bezier\CubicCurve;
use Alto\Bezier\Point;

$curve = new CubicCurve(
    new Point(0, 0),
    new Point(40, 160),
    new Point(160, -40),
    new Point(200, 120)
);

$mid = $curve->pointAt(0.5);
[$left, $right] = $curve->split(0.4);
$heading = $curve->tangent(0.5);
```

### Instantiate quadratic and N-order curves

```php
use Alto\Bezier\QuadraticCurve;
use Alto\Bezier\NOrderCurve;
use Alto\Bezier\Point;

$quadratic = new QuadraticCurve(
    new Point(0, 0),
    new Point(50, 120),
    new Point(120, 0)
);

$wave = new NOrderCurve(...array_map(
    fn(int $i) => new Point($i * 20, sin($i / 2) * 40 + 60),
    range(0, 7)
));
```

### Exporting control points to SVG

```php
$path = sprintf(
    'M %s C %s %s %s',
    ...array_map(fn(Point $p) => $p->x.' '.$p->y, $curve->toArray())
);
```

## Advanced

- **Arc length & constant-speed motion**: `length($samples)` returns the numerical arc length, while
  `pointAtDistance($distance, $samples)` maps real distances back to coordinates.
- **Intersections**: `$curveA->intersections($curveB, tolerance: 0.25, maxDepth: 18)` delivers intersection points along
  with their `t` parameters on each curve.
- **Numeric tuning**: increase sampling (default 100) for long or intricate paths to improve accuracy for arc-length and
  parameterization routines.
- **Bounding boxes and grids**: `boundingBox()` exposes width/height/corners, and `points($interval)` streams evenly
  spaced points for tessellation or hit-testing.

## Documentation

Read the [Alto Bezier documentation](https://altophp.com/bezier) for installation, curve types, operations, and
recipes.

## Testing

```bash
composer install
vendor/bin/phpstan analyse
vendor/bin/phpunit
```

## License

This project is licensed under the [MIT License](./LICENSE).
