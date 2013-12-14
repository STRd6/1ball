Hotkeys
=======

    Hotkeys = require "hotkeys"

    module.exports = (I, self) ->
      self.include Hotkeys

      hotkeys =
        r: ->
          self.restartLevel()
      
      for key, fn of hotkeys
        self.addHotkey key, fn
      
      return self