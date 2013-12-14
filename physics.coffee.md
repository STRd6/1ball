Physics
=======

    Collision = require "./lib/collision"

Stolen from Red Ice: https://raw.github.com/PixieEngine/RedIce/pixie/src/physics.coffee

    module.exports = (I={}, self) ->
      resolveCollision = (A, B) ->
        normal = B.center().subtract(A.center()).norm()
    
        # Elastic collisions
        relativeVelocity = A.velocity().subtract(B.velocity())
    
        massA = A.mass()
        massB = B.mass()
    
        totalMass = massA + massB
    
        pushA = normal.scale(-2 * (relativeVelocity.dot(normal) * (massB / totalMass)))
        pushB = normal.scale(+2 * (relativeVelocity.dot(normal) * (massA / totalMass)))
    
        # Adding impulse
        A.velocity A.velocity().add(pushA)
        B.velocity B.velocity().add(pushB)

      resolveCollisions = (objects) ->
        objects.eachPair (a, b) ->

          if Collision.circular(a.circle(), b.circle())
            resolveCollision(a, b)

      self.extend
        processPhysics: (objects, dt, steps=2) ->  
          dt /= steps
  
          steps.times ->
            objects.invoke "updatePosition", dt
      
            resolveCollisions(objects, dt)
