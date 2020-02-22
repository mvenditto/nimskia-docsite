# SkBitmap

## Read from file

```nim
proc readBitmapData(path: string): SKBitmap =
  var file = open(path, fmRead)
  let length = file.getFileSize()
  let buff = alloc(length)
  var s = newFileStream(file)
  assert s.readData(buff, length.int) > 0
  var data = newSkData(buff, length.int)
  result = decodeBitmap(newSkCodec(data))
  s.close()
```

## Read from stream

```nim
proc readBitmapStream(path: string): SKBitmap =
  var fs = newSkFileStream(path)
  assert fs.isValid
  var(res,codec) = newSkCodec(fs)
  assert res == Success
  result = decodeBitmap(codec)
```

## Drawing to a canvas

[*source*](https://github.com/mvenditto/nimskia/blob/master/nimskia/example/bitmap_basic.nim)

```nim
var bitmap = readBitmapStream("resources/images/skia.png")
let imageHeight = bitmap.info.height
canvas.drawBitmap(bitmap, 4, h / 2.0 - imageHeight.float / 2.0)
```

![](_images/sample__bitmap_basics.png ':size=256x256')

## Drawing and annotating

[*source*](https://github.com/mvenditto/nimskia/blob/master/nimskia/example/bitmap_annotation.nim)

```nim
var bitmap = readBitmapStream("resources/images/skia.png")

let paint = newSkPaint()
paint.strokeWidth = 4
paint.color = Red
paint.style = Stroke 

let annotCanvas = newSkCanvas(bitmap)
var rect = newSkRect(
  0, 
  0,
  bitmap.info.width.float,
  bitmap.info.height.float
)
annotCanvas.drawRect(rect, paint)
canvas.drawBitmap(bitmap,0,0)
```

![](_images/sample__bitmap_annotation.png ':size=256x256')