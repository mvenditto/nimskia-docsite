An overview a what the api looks like atm.

```nim

proc draw*(canvas: SKCanvas) =
    var fill = newPaint()
    fill.color = Blue
    canvas.drawPaint(fill)

    fill.color = Cyan
    var rect = newRect(100.float, 100, 540, 380)
    canvas.drawRect(rect, fill)

    var stroke = newPaint(Red)
    stroke.antialias = true
    stroke.style = Stroke
    stroke.strokeWidth = 5.0
    
    var path = newPath()
    discard path.moveTo(50.0, 50.0)
                .lineTo(590.0, 50.0)
                .cubicTo(-490.0, 50.0, 1130.0, 430.0, 50.0, 430.0)
                .lineTo(590.0, 430.0)

    canvas.drawPath(path, stroke)

    fill.color = newColorArgb(0x80, 0x00, 0xFF, 0x00)
    var rect2: SKRect = (120.0, 120.0, 520.0, 360.0)
    canvas.drawOval(rect2, fill)

    path.dispose()
    fill.dispose()
    stroke.dispose()

proc emitPng(path: string; surface: SKSurface) =
  var image = surface.snapshot()
  defer: image.dispose()
  
  var data = image.encode()
  defer: data.dispose()

  var f = open(path, fmWrite)
  var dataBuff = data.data
  var dataLen = len(data)
  discard f.writeBuffer(dataBuff, dataLen)
  f.close()

proc main() =
  var cs = newSrgbColorSpace()
  var info = newImageInfo(640, 480, Rgba_8888, Premul, cs)
  var surface = newRasterSurface(info)
  draw(surface.canvas)
  emitPng("out.png", surface);
```
nimskia + glfw (nimgl)

```nim
import nimgl/[glfw, opengl]
import ../nimskia/[
  sk_canvas, 
  sk_paint, 
  sk_surface, 
  sk_enums,
  sk_colors,
  gr_context
]
import common_api

proc keyProc(window: GLFWWindow, key: int32, scancode: int32,
             action: int32, mods: int32): void {.cdecl.} =
  if key == GLFWKey.ESCAPE and action == GLFWPress:
    window.setWindowShouldClose(true)

proc main() =
  assert glfwInit()

  glfwWindowHint(GLFWContextVersionMajor, 3)
  glfwWindowHint(GLFWContextVersionMinor, 3)
  glfwWindowHint(GLFWOpenglForwardCompat, GLFW_TRUE) 
  glfwWindowHint(GLFWOpenglProfile, GLFW_OPENGL_CORE_PROFILE)
  glfwWindowHint(GLFWResizable, GLFW_FALSE)

  let w: GLFWWindow = glfwCreateWindow(640, 480, "NimSkia")
  if w == nil:
    quit(-1)

  discard w.setKeyCallback(keyProc)
  w.makeContextCurrent()
  assert glInit()

  var grContext = createGL()
  assert not isNil grContext

  var info = newFrameBufferInfo(0, GL_RGBA8.uint32)
  assert not isNil info

  var target = createBackendRenderTarget(640, 480, 0, 0, info)
  assert not isNil target
 
  var surface = newSurface(
    grContext,
    target,
    TopLeft,
    Rgba8888,
    nil,
    nil
  )
  assert not isNil surface

  var canvas = surface.canvas
  assert not isNil canvas
  
  while not w.windowShouldClose:
    glfwPollEvents()
    testDraw(canvas)
    grContext.flush()
    w.swapBuffers()

  w.destroyWindow()
  glfwTerminate()

  grContext.abandonContext(true)
  surface.dispose()
  info.dispose()

main()
```

![logo](_images/nimskia_glfw.png ':size=320x240')
