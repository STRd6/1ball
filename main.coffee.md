1ine
====

Draw one line.

    require "./setup"
    Canvas = require "touch-canvas"
    {Size, Bounds} = require "./lib/util"

    size = Size
      width: 1024
      height: 576

    canvas = Canvas size

    $("body").append canvas.element()

    startPosition = null
    currentPosition = null
    canvas.on "touch", (position) ->
      startPosition = currentPosition = position.scale(size)

    canvas.on "move", (position) ->
      currentPosition = position.scale(size)

    canvas.on "release", (position) ->
      # TODO: Draw

      startPosition = currentPosition = null
    
    paint = ->
      canvas.fill("red")

      if startPosition
        canvas.drawCircle
          x: startPosition.x
          y: startPosition.y
          color: "blue"
          radius: 64

        canvas.drawLine
          start: startPosition
          end: currentPosition
          color: "black"

    setInterval ->
      paint()
    , 15
