Ball
====

The ball is what the player controls.

    GameObject = require "./game_object"

    module.exports = (I={}, self) ->
      Object.defaults I,
        color: "blue"
        radius: 64
        mass: 10

      self = GameObject(I)

      return self
