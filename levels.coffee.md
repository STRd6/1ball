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
    
    semiCircle = ->
      points = []

      16.circularPoints (p, i) ->
        p = p.scale(0.4).add(Point(1, 0.5))
        p.x = p.x * 576 / 1024
      
        if 0 < i < 8
          points.push p

      points

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
          Point(0.5, 0.25)
          Point(0.75, 0.5)
        ]
      deuce: ->
        map [
          Point(0.25, 0.1)
          Point(0.9, 0.9)
        ]
      four: ->
        map [
          Point(0.25, 0.5)
          Point(0.5, 0.25)
          Point(0.75, 0.5)
          Point(0.5, 0.75)
        ]
      ";)": ->
        map semiCircle().concat [
          Point(0.49, 0.25)
          Point(0.66, 0.25)
        ]
