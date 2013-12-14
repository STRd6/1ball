Ball
====

The ball is what the player controls.

    Compositions = require "./lib/compositions"

    module.exports = (I={}, self=Core(I)) ->
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

        update: (dt) ->
          self.position(self.position().add(self.velocity().scale(dt)))

      return self
