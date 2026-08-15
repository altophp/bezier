# Operations

Every curve class extends `Curve` and supports the same operations. The methods
return new values and never mutate the curve or its points.

## Evaluate and sample

`pointAt()` evaluates one parameter. Values from `0` to `1` cover the curve
from its start point to its end point.

```php
$point = $curve->pointAt(0.25);

foreach ($curve->points(0.1) as $sample) {
    // Use each Point instance.
}
```

`points()` accepts an interval greater than `0` and no greater than `1`. It
returns a generator, so samples do not need to be stored together.

## Split

```php
[$left, $right] = $curve->split(0.5);
```

The result contains two curves of the same concrete type. The end of `$left`
and the start of `$right` are the point at the split parameter.

## Direction

```php
$velocity = $curve->derivative(0.5);
$tangent = $curve->tangent(0.5);
$normal = $curve->normal(0.5);
```

`derivative()` returns the velocity vector. `tangent()` normalizes that vector,
and `normal()` rotates the tangent 90 degrees counterclockwise. A stationary
point produces zero tangent and normal vectors.

## Length and distance

Curve length is approximated numerically:

```php
$length = $curve->length(samples: 200);
$quarterT = $curve->parameterAtDistance($length / 4, samples: 200);
$quarterPoint = $curve->pointAtDistance($length / 4, samples: 200);
```

Use the same sample count when relating a length to a distance parameter.
Higher values improve the approximation and require more calculations.
`length()` requires at least two samples. Distance methods reject negative
distances and distances beyond the calculated curve length.

## Intersections

```php
$hits = $curve->intersections(
    $otherCurve,
    tolerance: 0.25,
    maxDepth: 18,
);

foreach ($hits as $hit) {
    $point = $hit['point'];
    $tOnFirst = $hit['t1'];
    $tOnSecond = $hit['t2'];
}
```

Intersection detection uses recursive subdivision and returns approximate
points. A smaller tolerance can refine the result but increases the work. The
maximum depth limits the subdivision search.

## Bounds

```php
$box = $curve->boundingBox();

$width = $box->width();
$height = $box->height();
$topLeft = $box->getTopLeft();
```

Quadratic and cubic curves calculate their extrema analytically. Higher-degree
curves return a conservative box derived from their control points and sampled
curve positions. The coordinate helpers `minX()`, `maxX()`, `minY()`, and
`maxY()` are useful for fast rejection tests.

## Export control points

```php
$coordinates = $curve->toArray();
```

`toArray()` returns the control points as `[x, y]` pairs. It does not sample or
flatten the curve.
