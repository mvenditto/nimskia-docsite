# SKCanvas

## autoRestore

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