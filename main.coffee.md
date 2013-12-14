1ball
====

Draw one line.

    require "./setup"
    Canvas = require "touch-canvas"
    {Size, Bounds} = require "./lib/util"
    Engine = require "./engine"

    FPS = 60
    dt = 1/FPS

    size = Size
      width: 1024
      height: 576

    engine = Engine
      size: size

    canvas = Canvas size

    $("body").append canvas.element()

    canvas.on "touch", engine.touch
    canvas.on "move", engine.move
    canvas.on "release", engine.release


    setInterval ->
      engine.update(dt)
      engine.draw(canvas)
    , dt * 1000
