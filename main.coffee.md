1ball
====

Draw one line.

    require "./setup"
    Music = require "./lib/music"
    Canvas = require "touch-canvas"
    {Size, Bounds} = require "./lib/util"
    Engine = require "./engine"

    FPS = 60
    dt = 1/FPS

    Music.play "music"

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

    mainLoop = ->
      engine.update(dt)
      engine.draw(canvas)
      
      requestAnimationFrame mainLoop

    requestAnimationFrame mainLoop