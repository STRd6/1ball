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

    immobile = (pins) ->
      pins.map (p) ->
        p.I.color = "black"
        p.I.immobile = true

        p

    incorporeal = (pins) ->
      pins.map (p) ->
        p.I.color = "rgba(255, 255, 255, 0.25)"
        p.I.incorporeal = true

        p

    invisible = (pins) ->
      pins.map (p) ->
        p.I.color = "transparent"

        p

    restoreAspect = (p) ->
      p.x = p.x * 576 / 1024 + 0.21875

      p
    
    big = (positions) ->
      positions.map (p) ->
        Pin
          position: p.scale(size)
          radius: 50
          mass: 4
    
    circle = (n, scale=0.4, ellipse=false) ->
      points = []
      n.circularPoints (p) ->
        p = p.scale(scale).add(Point(0.5, 0.5))

        if ellipse
          points.push p
        else
          points.push restoreAspect p

      return points

    semiCircle = ->
      points = []

      16.circularPoints (p, i) ->
        p = p.scale(0.4).add(Point(0.5, 0.5))

        if 0 < i < 8
          points.push restoreAspect p

      points

    module.exports = [{ # Chapter 1
      "⚀": ->
        big [
          Point(0.5, 0.5)
        ]
      "⋯": ->
        big [
          Point(0.25, 0.5)
          Point(0.5, 0.5)
          Point(0.75, 0.5)
        ]
      "⚁": ->
        big [
          Point(0.25, 0.75)
          Point(0.75, 0.25)
        ].map restoreAspect
      "⚂": ->
        big [
          Point(0.25, 0.75)
          Point(0.5, 0.5)
          Point(0.75, 0.25)
        ].map restoreAspect
      "⋮": ->
        big [
          Point(0.5, 0.25)
          Point(0.5, 0.5)
          Point(0.5, 0.75)
        ]
      "♢": ->
        big [
          Point(0.25, 0.5)
          Point(0.5, 0.25)
          Point(0.75, 0.5)
          Point(0.5, 0.75)
        ].map restoreAspect
      "⊿": ->
        big [
          Point(0.75, 0.25)
          Point(0.25, 0.75)
          Point(0.75, 0.75)
        ].map restoreAspect
      "⚃": ->
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
      "⚀": ->
        small [
          Point(0.5, 0.5)
        ]
      "⋯": ->
        small [
          Point(0.25, 0.5)
          Point(0.5, 0.5)
          Point(0.75, 0.5)
        ]
      "⚁": ->
        small [
          Point(0.25, 0.75)
          Point(0.75, 0.25)
        ].map restoreAspect
      "3": ->
        small [
          Point(0.25, 0.5)
          Point(0.5, 0.25)
          Point(0.75, 0.5)
        ]
      "⋮": ->
        small [
          Point(0.5, 0.25)
          Point(0.5, 0.5)
          Point(0.5, 0.75)
        ]
      "♢": ->
        small [
          Point(0.25, 0.5)
          Point(0.5, 0.25)
          Point(0.75, 0.5)
          Point(0.5, 0.75)
        ].map restoreAspect
      "⊿": ->
        small [
          Point(0.75, 0.25)
          Point(0.25, 0.75)
          Point(0.75, 0.75)
        ].map restoreAspect
      "⬡": ->
        small circle(6)
    }, { # Immobile Chapter
      "⚀": ->
        immobile small [
          Point(0.5, 0.5)
        ]
      "⚁": ->
        immobile small [
          Point(0.25, 0.75)
          Point(0.75, 0.25)
        ].map restoreAspect
      "3": ->
        immobile small [
          Point(0.25, 0.5)
          Point(0.5, 0.25)
          Point(0.75, 0.5)
        ]
      "♢": ->
        immobile small [
          Point(0.25, 0.5)
          Point(0.5, 0.25)
          Point(0.75, 0.5)
          Point(0.5, 0.75)
        ].map restoreAspect
      "⊿": ->
        immobile small [
          Point(0.75, 0.25)
          Point(0.25, 0.75)
          Point(0.75, 0.75)
        ].map restoreAspect
      "line": ->
        immobile small [
          Point(0.25, 0.5)
          Point(0.75, 0.5)
        ].map restoreAspect
      "⬡": ->
        immobile small circle(6)
    }, { # Incorporeal
      "⚀": ->
        incorporeal big [
          Point(0.5, 0.5)
        ]
      "⋯": ->
        incorporeal big [
          Point(0.25, 0.5)
          Point(0.5, 0.5)
          Point(0.75, 0.5)
        ]
      "⚁": ->
        incorporeal big [
          Point(0.25, 0.75)
          Point(0.75, 0.25)
        ].map restoreAspect

      "⚂": ->
        incorporeal big [
          Point(0.25, 0.75)
          Point(0.5, 0.5)
          Point(0.75, 0.25)
        ].map restoreAspect

      ">": ->
        incorporeal(big [
          Point(0.25, 0.25)
          Point(0.25, 0.75)
        ]).concat immobile small [
          Point(0.75, 0.5)
        ]

      ">2": ->
        incorporeal(big [
          Point(0.25, 0.25)
          Point(0.25, 0.75)
        ]).concat immobile small [
          Point(0.75, 0.5)
          Point(0.15, 0.15)
          Point(0.15, 0.85)
        ]
    }, { # Invisible chapter
      "⚀": ->
        invisible big [
          Point(0.5, 0.5)
        ]

      "⋯": ->
        invisible big [
          Point(0.25, 0.5)
          Point(0.5, 0.5)
          Point(0.75, 0.5)
        ]

      "⚁": ->
        invisible big [
          Point(0.25, 0.75)
          Point(0.75, 0.25)
        ].map restoreAspect

      "⚂": ->
        invisible big [
          Point(0.25, 0.75)
          Point(0.5, 0.5)
          Point(0.75, 0.25)
        ].map restoreAspect

      "⋮": ->
        invisible big [
          Point(0.5, 0.25)
          Point(0.5, 0.5)
          Point(0.5, 0.75)
        ]

      "♢": ->
        invisible big [
          Point(0.25, 0.5)
          Point(0.5, 0.25)
          Point(0.75, 0.5)
          Point(0.5, 0.75)
        ].map restoreAspect

      "⊿": ->
        invisible big [
          Point(0.75, 0.25)
          Point(0.25, 0.75)
          Point(0.75, 0.75)
        ].map restoreAspect

      "⚃": ->
        invisible big [
          Point(0.25, 0.25)
          Point(0.75, 0.25)
          Point(0.25, 0.75)
          Point(0.75, 0.75)
        ].map restoreAspect

      "⬡": ->
        positions = circle(6)
        positions.splice(1, 1)

        invisible big positions
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

      "⬭": -> # Not solved yet
        small circle(24, 0.3, true)

      "⚃": -> # This one is tough!
        small [
          Point(0.25, 0.25)
          Point(0.75, 0.25)
          Point(0.25, 0.75)
          Point(0.75, 0.75)
        ].map restoreAspect
    }]
