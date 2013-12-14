Levels
======

    Pin = require "./pin"

    module.exports = {
      test: ->
        pins = []

        10.times (x) ->
          10.times (y) ->
            pins.push Pin
              position:
                x: 50 * x
                y: 50 * y

        return pins
    }
