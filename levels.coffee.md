Levels
======

    Pin = require "./pin"
    {Size} = require "./lib/util"
      
    size = Size
      width: 1024
      height: 576

    map = (positions) ->
      positions.map (p) ->
        Pin
          position: p.scale(size)

    module.exports =
      one: ->
        map [
          Point(0.75, 0.5)
        ]
      two: ->
        map [
          Point(0.25, 0.5)
          Point(0.75, 0.5)
        ]
      three: ->
        map [
          Point(0.25, 0.5)
          Point(0.33, 0.33)
          Point(0.75, 0.5)
        ]
      deuce: ->
        map [
          Point(0.25, 0.25)
          Point(0.75, 0.75)
        ]
