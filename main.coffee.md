1ball
====

Draw one line.

    require "./setup"
    Canvas = require "touch-canvas"
    {Size, Bounds} = require "./lib/util"

    FPS = 60
    dt = 1000/FPS

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
      target = position.scale(size)

      fire startPosition, getPower(startPosition, target)

      startPosition = currentPosition = null

    fire = (start, power) ->
      console.log start, power

    update = ->

    getPower = (a, b) ->
      p = Point.distance(a, b) / 200
      
      p.clamp 0, 1

    draw = ->
      canvas.fill("red")

      if startPosition
        power = getPower(startPosition, currentPosition)
        
        canvas.drawCircle
          x: startPosition.x
          y: startPosition.y
          color: "blue"
          radius: 64

        canvas.drawLine
          start: startPosition
          end: currentPosition
          color: "black"
        
        fontSize = 32 + 6 * Math.sin(t * 1.5 * Math.TAU)
        canvas.font "#{fontSize}px bold 'HelveticaNeue-Light', 'Helvetica Neue Light', 'Helvetica Neue', Helvetica, Arial, 'Lucida Grande', sans-serif"
        canvas.centerText
          color: "white"
          position: currentPosition
          text: (power * 100).toFixed(2)

    t = 0
    setInterval ->
      update()
      draw()
      t += dt/1000
    , dt
