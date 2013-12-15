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

    module.exports = (I={}) ->
      Object.defaults I,
        hit: false

      self = GameObject(I)

      self.attrAccessor "hit"

      self.extend
        collided: (other) ->
          self.hit true
          self.color "yellow"

      return self
