Setup
=====

Set up our runtime styles and expose some stuff for debugging.

    # For debug purposes
    global.PACKAGE = PACKAGE
    global.require = require

    runtime = require("runtime")(PACKAGE)
    runtime.boot()
    runtime.applyStyleSheet(require('./style'))

    # Updating Application Cache and prompting for new version
    require "appcache"

    require "jquery-utils"

    window.requestAnimationFrame = 
      window.requestAnimationFrame or 
      window.mozRequestAnimationFrame or
      window.webkitRequestAnimationFrame or 
      window.msRequestAnimationFrame
