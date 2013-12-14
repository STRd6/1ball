Engine
======

    Compositions = require "./lib/compositions"
    Hotkeys = require "./hotkeys"

    Ball = require "./ball"
    Levels = require "./levels"
    Pin = require "./pin"
    Physics = require "./physics"
    {Size} = require "./lib/util"

    module.exports = (I={}, self=Core(I)) ->
      Object.defaults I,
        age: 0
        levelName: "test"
        pins: [
          {}
        ]
        startZone: 100

      self.include Compositions
      self.include Hotkeys
      self.include Physics

      self.attrAccessor "age", "levelName"

      self.attrModel "ball", Ball
      self.attrModel "size", Size
      self.attrModels "pins", Pin
      
      instructions = """
        Click and Drag
        
        'r' to Restart Level
      """

      startPosition = currentPosition = null

      getPower = (a, b) ->
        p = Point.distance(a, b) / 200
        
        p.clamp 0, 1

      gameOver = false

      self.extend
        resetLevel: ->
          gameOver = false
          self.ball(null)

          # Reset pins
          self.pins Levels[self.levelName()]()

        goToLevel: (n) ->
          

        touch: (position) ->
          unless self.ball()
            startPosition = currentPosition = position.scale(self.size())
            
            startPosition.x = startPosition.x.clamp(0, I.startZone)

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
          unless gameOver
            self.processPhysics(self.objects(), dt)
  
            I.age += dt
  
            done = self.pins().reduce (done, pin) ->
              done and pin.hit()
            , true

            if done
              gameOver = true
              alert "A winner is you"

        draw: (canvas) ->
          canvas.fill("red")
          canvas.fill
            x: 0
            y: 0
            width: I.startZone
            height: self.size().height
            color: "gray"

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
          else if self.ball()
            self.ball().draw canvas
          else if mousePosition
            canvas.drawCircle
              x: mousePosition.x
              y: mousePosition.y
              color: "blue"
              radius: 64
          
          self.pins.forEach (pin) ->
            pin.draw(canvas)


          if I.age < 5
            instructions.split("\n").map (text, i) ->
              canvas.font "40px bold 'HelveticaNeue-Light', 'Helvetica Neue Light', 'Helvetica Neue', Helvetica, Arial, 'Lucida Grande', sans-serif"

              canvas.centerText
                color: "pink"
                position: Point(0.5, 0.5).scale(self.size()).add Point(0, 50 * (i - 1) )
                text: text

      # Hack for gosting on pointer devices
      mousePosition = null

      $("body").on "mousemove", (e) ->
        mousePosition = localPosition(e)
        mousePosition.x = mousePosition.x.clamp(0, I.startZone)
      
      localPosition = (e) ->
        $currentTarget = $("canvas")
        offset = $currentTarget.offset()
    
        point = Point(
          (e.pageX - offset.left)
          (e.pageY - offset.top)
        )
    
        return point

      self.resetLevel()

      return self
