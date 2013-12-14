1ball
====

Draw one line.

    require "./setup"
    Canvas = require "touch-canvas"
    Compositions = require "./lib/compositions"
    {Size, Bounds} = require "./lib/util"

    FPS = 60
    dt = 1/FPS

    size = Size
      width: 1024
      height: 576

    canvas = Canvas size

    Ball = (I={}, self=Core(I)) ->
      Object.defaults I,
        color: "blue"
        position:
          x: 0
          y: 0
        velocity:
          x: 0
          y: 0
        radius: 64

      self.include Compositions

      self.attrModel "position", Point
      self.attrModel "velocity", Point

      self.extend
        draw: (canvas) ->
          p = self.position()
          canvas.drawCircle
            x: p.x
            y: p.y
            color: I.color
            radius: I.radius

        update: ->
          self.position(self.position().add(self.velocity().scale(dt)))

      return self

    $("body").append canvas.element()

    startPosition = null
    currentPosition = null
    canvas.on "touch", (position) ->
      startPosition = currentPosition = position.scale(size)

    canvas.on "move", (position) ->
      currentPosition = position.scale(size)

    canvas.on "release", (position) ->
      currentPosition = position.scale(size)

      fire startPosition, currentPosition

      startPosition = currentPosition = null

    ball = null

    update = ->
      ball?.update()

    fire = (start, target) ->
      ball = Ball
        position: start
        velocity: target.subtract(start).norm getPower(start, target) * 1600

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

      if ball
        ball.draw canvas

    t = 0
    setInterval ->
      update()
      draw()
      t += dt
    , dt * 1000
