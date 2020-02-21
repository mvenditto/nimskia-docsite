# SKCanvas

## `drawVertices`

[*source*](https://github.com/mvenditto/nimskia/blob/cc7dd1c9fbfa047631c02da501d81d1731e77f2e/nimskia/example/vertex_drawing.nim#L1)

### Drawing a triangle

```nim
proc drawTriangle(canvas: SKCanvas, paint: SKPaint) =

  let r = 65.0
  let(cx,cy) = (hw * 0.5, hh * 0.5)

  let vertices = [
    (cx + r, cy - r).SKPoint,
    (cx - r, cy - r),
    (cx, cy + r)
  ]

  let colors = @[
    Red,
    Green,
    Blue
  ]

  canvas.drawVertices(Triangles, vertices, colors, paint)
```

### Drawing a box

```nim
proc drawBox(canvas: SKCanvas, paint: SKPaint) =

  let r = 65.0
  let(cx,cy) = (hw * 1.5, hh * 0.5)

  let vertices = [
    (cx - r, cy + r).SKPoint,
    (cx + r, cy + r),
    (cx + r, cy - r),
    (cx - r, cy - r),
  ]

  let colors = @[
    Red,
    Yellow,
    Blue,
    Violet
  ]

  canvas.drawVertices(TriangleFan, vertices, colors, paint)
```

### Drawing a textured triangle

```nim
proc drawTexturedTriangle(canvas: SKCanvas, paint: SKPaint) =

  let r = 65.0
  let(cx,cy) = (hw * 0.5, hh * 1.5)

  let vertices = [
    (cx + r, cy - r).SKPoint,
    (cx - r, cy - r).SKPoint,
    (cx, cy + r).SKPoint
  ]

  let(tw,th) = (512.0, 512.0)

  let texs = [ 
    (0.0, 0.0).SKPoint,
    (0.0, th),
    (tw, th)
  ]

  let verts = copy(TriangleFan, vertices, texs)
  
  canvas.drawVertices(verts, Modulate, paint)
```

### Drawing a textured box

```nim
proc drawTexturedBox(canvas: SKCanvas, paint: SKPaint) =
  
  let r = 65.0
  let(cx,cy) = (hw * 1.5, hh * 1.5)

  let vertices = [
    (cx - r, cy + r).SKPoint,
    (cx + r, cy + r),
    (cx + r, cy - r),
    (cx - r, cy - r),
  ]

  let(tw,th) = (512.0, 512.0)

  let texs = [ 
    (0.0, 0.0).SKPoint,
    (0.0, th),
    (tw, th),
    (0.0, th)
  ]

  let colors = @[
    Red,
    Yellow,
    Blue,
    Violet
  ]

  let verts = copy(TriangleFan, vertices, texs, colors)
  
  canvas.drawVertices(verts, Modulate, paint)
```

```nim
let bmp = decodeBitmap("resources/images/plasma.png")
paint.shader = newBitmapShader(bmp)
```

### Drawing a textured hexagon

```nim
proc drawTexturedExagon(canvas: SKCanvas, paint: SKPaint) =

  var r = 64.0
  var hr = 32.0
  var(cx, cy) = (hw, hh)

  let vertices = [
    (cx - hr, cy + r).SKPoint,
    (cx - r,  cy),
    (cx - hr, cy - r),
    (cx + hr, cy - r),
    (cx + r,  cy),
    (cx + hr, cy + r),
  ]

  let colors = @[
    Green,Yellow,White,Violet,Red,Cyan
  ]

  let indices = @[
   0.uint16,1,2,3,4,5
  ]

  r = 512
  hr = 256
  (cx, cy) = (256.0, 256.0)

  let texs = [
    (hr, 0.0).SKPoint,
    (0.0, hr),
    (hr, r),
    (r, r),
    (r - hr, r),
    (r, 0.0)
  ]

  let verts = copy(TriangleFan, vertices, texs, colors, indices)

  canvas.drawVertices(verts, Modulate, paint)
```

![](_images/sample__vertex_drawing.png ':size=256x256')

## `autoRestore`
[*source*](https://github.com/mvenditto/nimskia/blob/master/nimskia/example/canvas_autorestore.nim)

```nim
const str = "R"

var textBounds = new(SKRect)
paint.measureText(str, textBounds)

let tx = hw - textBounds.width / 2
let ty = hh + textBounds.height / 2

canvas.clear(White)
paint.color = Gray

canvas.autoRestore:
  canvas.translate(tx,ty)
  canvas.skew(-1.5, 0.0)
  canvas.translate(-tx,-ty)
  canvas.drawText(str, tx, ty, paint)

paint.color = Black
canvas.drawText(str, tx, ty, paint)
```

![](_images/sample__canvas_autoRestore.png ':size=256x256')