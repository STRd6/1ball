Levels
======

    Pin = require "./pin"
    {Size} = require "./lib/util"
      
    size = Size
      width: 1024
      height: 576
      
    module.exports =
      test: ->
        [
          Point(0.25, 0.5)
          Point(0.75, 0.5)
        ].map (p) ->
          position: p.scale(size)
    
    module.exports = {
      test: ->
        pins = []

        2.times (x) ->
          1.times (y) ->
            pins.push Pin
              position:
                x: 250 * (x - 0.5) + size.width / 2
                y: 250 * (x - 0.5) + size.height / 2

        return pins
    }
