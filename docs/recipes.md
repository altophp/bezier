# Recipes

These recipes combine the shared curve operations into common application
tasks.

## Import an SVG segment

SVG quadratic and cubic path segments map directly to their dedicated curve
classes. A cubic segment needs the current path position, two controls, and the
segment end:

```php
use Alto\Bezier\CubicCurve;
use Alto\Bezier\Point;

$curve = new CubicCurve(
    new Point($currentX, $currentY),
    new Point($control1X, $control1Y),
    new Point($control2X, $control2Y),
    new Point($endX, $endY),
);
```

SVG does not provide native quartic, quintic, or arbitrary-degree path
commands. Sample those curves or convert them to segments supported by the
target renderer.

## Animate at constant speed

Equal steps in `t` do not usually represent equal distances. Calculate the
length once, then request positions by distance:

```php
$samples = 200;
$frameCount = 20;
$length = $curve->length($samples);
$frames = [];

for ($frame = 0; $frame <= $frameCount; ++$frame) {
    $distance = $length * $frame / $frameCount;
    $frames[] = $curve->pointAtDistance($distance, $samples);
}
```

The first and last entries are the curve endpoints. Intermediate entries are
approximately equally spaced along the path.

## Reject distant curves before intersecting

Intersection detection already checks bounds during subdivision. An
application comparing many curves can reject non-overlapping pairs first:

```php
$a = $curve->boundingBox();
$b = $otherCurve->boundingBox();

$separate = $a->maxX() < $b->minX()
    || $b->maxX() < $a->minX()
    || $a->maxY() < $b->minY()
    || $b->maxY() < $a->minY();

$hits = $separate ? [] : $curve->intersections($otherCurve);
```

## Fit a curve into a viewport

Use the bounding box to derive a translation and scale for a renderer:

```php
$box = $curve->boundingBox();
$scale = min(
    $viewportWidth / $box->width(),
    $viewportHeight / $box->height(),
);

$offsetX = -$box->minX() * $scale;
$offsetY = -$box->minY() * $scale;
```

Handle zero-width or zero-height boxes before dividing when a curve can be
degenerate or perfectly horizontal or vertical.
