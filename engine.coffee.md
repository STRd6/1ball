Engine
======
    Compositions = require "./lib/compositions"
    Hotkeys = require "./hotkeys"

    Ball = require "./ball"
    Pin = require "./pin"
    Physics = require "./physics"
    {Size} = require "./lib/util"

    module.exports = (I={}, self=Core(I)) ->
      Object.defaults I,
        age: 0
        pins: [
          {
            position:
              x: 200
              y: 200
          }
        ]

      self.include Compositions
      self.include Hotkeys
      self.include Physics

      self.attrAccessor "age"

      self.attrModel "ball", Ball
      self.attrModel "size", Size
      self.attrModels "pins", Pin
      
      instructions = """
        Click and Drag
        
        'r' to Restart Level
      """

      pins = []
      10.times (x) ->
        10.times (y) ->
          pins.push Pin
            position:
              x: 50 * x
              y: 50 * y

      self.pins pins

      startPosition = currentPosition = null

      getPower = (a, b) ->
        p = Point.distance(a, b) / 200
        
        p.clamp 0, 1

      self.extend
        restartLevel: ->
          self.ball(null)

          # TODO: Reset pins

        goToLevel: (n) ->
          

        touch: (position) ->
          unless self.ball()
            startPosition = currentPosition = position.scale(self.size())

        move: (position) ->
          currentPosition = position.scale(self.size())

        release: (position) ->
          if startPosition
            currentPosition = position.scale(self.size())
      
            self.fire startPosition, currentPosition
      
            startPosition = currentPosition = null

        fire: (start, target) ->
          self.ball Ball
            position: start
            velocity: target.subtract(start).norm getPower(start, target) * 1600 + 1

        objects: ->
          if self.ball()
            self.pins().concat(self.ball())
          else
            self.pins()

        update: (dt) ->
          self.processPhysics(self.objects(), dt)

          I.age += dt

        draw: (canvas) ->
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
            
            fontSize = 32 + 6 * Math.sin(self.age() * 1.5 * Math.TAU)
            canvas.font "#{fontSize}px bold 'HelveticaNeue-Light', 'Helvetica Neue Light', 'Helvetica Neue', Helvetica, Arial, 'Lucida Grande', sans-serif"
            canvas.centerText
              color: "white"
              position: currentPosition
              text: (power * 100).toFixed(2)

          self.pins.forEach (pin) ->
            pin.draw(canvas)

          if self.ball()
            self.ball().draw canvas
