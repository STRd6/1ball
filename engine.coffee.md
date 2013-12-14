Engine
======
    Compositions = require "./lib/compositions"
    Hotkeys = require "./hotkeys"

    Ball = require "./ball"
    {Size} = require "./lib/util"

    module.exports = (I={}, self=Core(I)) ->
      Object.defaults I,
        age: 0

      self.include Compositions
      self.include Hotkeys

      self.attrAccessor "age"

      self.attrModel "ball", Ball
      self.attrModel "size", Size

      startPosition = currentPosition = null

      getPower = (a, b) ->
        p = Point.distance(a, b) / 200
        
        p.clamp 0, 1

      self.extend
        restartLevel: ->
          self.ball(null)

          # TODO: Reset pins
          
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

        update: (dt) ->
          self.ball()?.update(dt)

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
    
          if self.ball()
            self.ball().draw canvas
