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

    small = map

    restoreAspect = (p) ->
      p.x = p.x * 576 / 1024 + 0.21875

      p
    
    big = (positions) ->
      positions.map (p) ->
        Pin
          position: p.scale(size)
          radius: 50
          mass: 4
    
    circle = (n, scale=0.4) ->
      points = []
      n.circularPoints (p) ->
        points.push restoreAspect p.scale(scale).add(Point(0.5, 0.5))

      return points

    semiCircle = ->
      points = []

      16.circularPoints (p, i) ->
        p = p.scale(0.4).add(Point(0.5, 0.5))

        if 0 < i < 8
          points.push restoreAspect p

      points

    module.exports = [{ # Chapter 1
      one: ->
        big [
          Point(0.75, 0.5)
        ]
      two: ->
        big [
          Point(0.25, 0.5)
          Point(0.75, 0.5)
        ]
      deuce: ->
        big [
          Point(0.25, 0.1)
          Point(0.9, 0.9)
        ]
      three: ->
        big [
          Point(0.25, 0.5)
          Point(0.5, 0.25)
          Point(0.75, 0.5)
        ]
      line: ->
        big [
          Point(0.5, 0.25)
          Point(0.5, 0.75)
        ]
      four: ->
        big [
          Point(0.25, 0.5)
          Point(0.5, 0.25)
          Point(0.75, 0.5)
          Point(0.5, 0.75)
        ]
      triangle: ->
        big [
          Point(0.75, 0.25)
          Point(0.25, 0.75)
          Point(0.75, 0.75)
        ].map restoreAspect
      "□": ->
        big [
          Point(0.25, 0.25)
          Point(0.75, 0.25)
          Point(0.25, 0.75)
          Point(0.75, 0.75)
        ].map restoreAspect

      "⬡": ->
        positions = circle(6)
        positions.splice(1, 1)

        big positions
    }, { # Chapter 2
      one: ->
        small [
          Point(0.75, 0.5)
        ]
      two: ->
        small [
          Point(0.25, 0.5)
          Point(0.75, 0.5)
        ]
      deuce: ->
        small [
          Point(0.25, 0.1)
          Point(0.9, 0.9)
        ]
      three: ->
        small [
          Point(0.25, 0.5)
          Point(0.5, 0.25)
          Point(0.75, 0.5)
        ]
      line: ->
        small [
          Point(0.5, 0.25)
          Point(0.5, 0.75)
        ]
      four: ->
        small [
          Point(0.25, 0.5)
          Point(0.5, 0.25)
          Point(0.75, 0.5)
          Point(0.5, 0.75)
        ]
      triangle: ->
        small [
          Point(0.75, 0.25)
          Point(0.25, 0.75)
          Point(0.75, 0.75)
        ].map restoreAspect
      "□": -> # This one is tough!
        small [
          Point(0.25, 0.25)
          Point(0.75, 0.25)
          Point(0.25, 0.75)
          Point(0.75, 0.75)
        ].map restoreAspect

      "⬡": ->
        small circle(6)
    }, { # Chapter 3
      ":)": ->
        map(semiCircle()).concat big [
          Point(0.4, 0.33)
          Point(0.6, 0.33)
        ]
      "◎": ->
        map(circle(24)).concat(big [Point(0.5, 0.5)])
      "✝": ->
        map [
          Point(0.5, 0.35)
          Point(0.35, 0.5)
          Point(0.5, 0.5)
          Point(0.65, 0.5)
          Point(0.5, 0.65)
          Point(0.5, 0.8)
        ].map restoreAspect
      
      "○": ->
        map circle(24)
    }]
