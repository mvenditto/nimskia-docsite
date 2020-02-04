# SKSurface

## Save as image

```nim
proc emitPng*(path: string, surface: SKSurface) =
  var image = surface.snapshot()
  defer: image.dispose()
  
  var data = image.encode()
  defer: data.dispose()

  var f = open(path, fmWrite)
  var dataBuff = data.data
  var dataLen = len(data)
  discard f.writeBuffer(dataBuff, dataLen)
  f.close()
```
