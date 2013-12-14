Game Object
===========

    Compositions = require "./lib/compositions"

    module.exports = (I={}, self=Core(I)) ->
      Object.defaults I,
        color: "white"
        velocity:
          x: 0
          y: 0
        position:
          x: 0
          y: 0
        radius: 16
        mass: 1

      self.include Compositions

      self.attrAccessor "color", "mass", "radius"

      self.attrModel "velocity", Point
      self.attrModel "position", Point

      self.extend
        draw: (canvas) ->
          p = self.position()
          canvas.drawCircle
            x: p.x
            y: p.y
            color: self.color()
            radius: self.radius()

        update: (dt) ->

        updatePosition: (dt) ->
          newPosition = self.position().add(self.velocity().scale(dt))
          self.position(newPosition)
          I.x = newPosition.x
          I.y = newPosition.y

        center: self.position

        circle: ->
          I

      return self
