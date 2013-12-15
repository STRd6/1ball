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

    osscilate = (pins, deltaPhi=0.1) ->
      pins.map (p, i) ->
        p.I.osscilate = true
        p.I.phi = i * deltaPhi

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

      "○": ->
        big circle(12)
        
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
      "three": ->
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
      "○": ->
        small circle(30)
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
      "three": ->
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

      "Y": ->
        incorporeal(big [
          Point(0.3, 0.5)
          Point(0.8, 0.2)
          Point(0.8, 0.8)
        ]).concat big [
          Point(0.6, 0.5)
        ]

      "⚃": ->
        incorporeal(big [
          Point(0.25, 0.25)
          Point(0.25, 0.75)
        ].map restoreAspect).concat immobile big [
          Point(0.75, 0.25)
          Point(0.75, 0.75)
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

      "○": ->
        invisible big circle(12)

      "⚀?": ->
        big([
          Point(0.5, 0.5)
        ]).concat invisible incorporeal big [
          Point(0.75, 0.75)
        ]
    }, { # Ossilators
      "|": ->
        osscilate incorporeal big [
          Point(0.5, 0.5)
        ]

      "||": ->
        osscilate incorporeal(big [
          Point(0.75, 0.5)
          Point(0.25, 0.5)
        ]), -1
      "☆NSYNC": ->
        osscilate incorporeal(big [
          Point(0.75, 0.5)
          Point(0.25, 0.5)
        ]), 0
      "|||": ->
        osscilate incorporeal(big [
          Point(0.75, 0.5)
          Point(0.5, 0.5)
          Point(0.25, 0.5)
        ]), -1

      "∿": ->
        osscilate incorporeal(big [
          Point(0.3, 0.5)
          Point(0.4, 0.5)
          Point(0.5, 0.5)
          Point(0.6, 0.5)
          Point(0.7, 0.5)
        ]), -0.2
      "☄": ->
        osscilate(incorporeal(big [
          Point(0.75, 0.5)
          Point(0.75, 0.5)
          Point(0.5, 0.5)
          Point(0.5, 0.5)
          Point(0.25, 0.5)
          Point(0.25, 0.5)
        ]), -1).concat small [
          Point(0.35, 0.5)
        ]
    }, { # Bonus town
      ":)": ->
        map(semiCircle()).concat big [
          Point(0.4, 0.33)
          Point(0.6, 0.33)
        ]

      "◎": ->
        map(circle(24)).concat(big [Point(0.5, 0.5)])

      "☆NSYNC Reunion Tour 2014?": ->
        osscilate(incorporeal(big [
          Point(0.75, 0.5)
          Point(0.25, 0.5)
        ]), 0).concat invisible incorporeal small [
          Point(0.9, 0.9)
        ]

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
        pins = immobile small circle(30)
        pins.splice(14, 3)

        pins

      "⬡": ->
        small circle(6)

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

    scraps =
      "☈": ->
        immobile(small [
          Point(0.9, 0.5)
        ]).concat big [
          Point(0.2, 0.2)
          Point(0.2, 0.8)
          Point(0.8, 0.2)
          Point(0.8, 0.8)
        ].map restoreAspect
