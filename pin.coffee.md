Pin
===

The player must activate all pins to complete the level.

Pins are activated by being struck with the ball or another pin.

Types
-----

Normal - Have mass and physics
Big Ones
Light - Must be triggered by overlap, but don't have any physical effects
Invisible
Bumper - Immovable
Exploders - Split into multiple
Portal - Transports entities through it

    GameObject = require "./game_object"
    Sound = require "./lib/sound"

    module.exports = (I={}) ->
      Object.defaults I,
        age: 0
        hit: false
        phi: 0
        osscilate: false
        omega: 0.5

      self = GameObject(I)

      self.attrAccessor "hit"

      self.extend
        collided: (other) ->
          self.hit true
          self.color "yellow"

          Sound.play("hit#{rand(4)}")

        update: (dt) ->
          I.age += dt

          if I.osscilate and !I.hit
            self.position().y = (Math.sin((I.age + I.phi) * Math.TAU * I.omega) * 0.5 + 0.5) * 576

      return self
