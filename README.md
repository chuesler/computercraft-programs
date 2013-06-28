computercraft-programs
======================

A couple of programs for <a href="http://www.computercraft.info/">ComputerCraft</a>, a mod for Minecraft. No guarantees.

Tested with ComputerCraft v1.53.

mobfarm
-------
The set of programs in the mobfarm directory are for a <a href="http://www.minecraftforum.net/topic/1475847-152">soulshards</a>-Style mob farm. Basically: You have a (rapid) source of mobs and a couple of Melee Turtles farming them.

The melee turtles can be run standalone, however, you can also add another computer to support self-updating of the codebase. This was mainly useful while adjusting the code, so I didn't have to change the same code on 7 turtles.

mining
------
These programs allow you to create a quarry of sorts. This is done with the help of a couple of mods: <a href="http://www.mod-buildcraft.com/">Buildcraft</a> (for the Mining Well), <a href="http://thermalexpansion.wikispaces.com/">Thermal Expansion</a> (Tesseracts, Redstone Energy Conduits, Wrench/Engineering Turtle) and some form of transport for the mined items (Ender Chests are a good one, either vanilla or <a href="http://www.minecraftforum.net/topic/909223-147152">Ender Storage</a>). You also need some form of chunkloading (I recommend <a href="http://www.minecraftforum.net/topic/909223-147152">ChickenChunks</a>). I suppose you could use railcraft chunkloaders as well, but you'd have to feed them with Ender Pearls.

Note that this is not an early game build; you need a robust inventory system to manage the rapid influx of items as well as a sizable amount of buildcraft power.

There are 4 different sets of programs here. All turtles require a Wireless Modem on the right side and either a Crescent Hammer (Wrench Turtle) or a Diamond Pick (Mining Turtle) on the left.
On all turtles except the command turtle, you need to adjust the first line of the programs (local serverId = ...). The correct id value can be found by executing "id" on the command turtle.
I also recommend labeling all the turtles by using "label set ..." after placing them.

* command: Wrench Turtle
  This turtle supplies power using an energy tesseract and, more importantly, sends commands to all the other turtles. This is done using <a href="http://computercraft.info/wiki/Rednet.broadcast">rednet broadcast messages</a>. This turtle can also update all the mining turtles' code. There can only be one of these (see energy below if you want higher throughput).
  
  Inventory setup: Shiny Dust'ed Item Tesseract supplied with fuel for the turtle in slot 14, 1 Redstone Energy Conduit in slot 15, and 1 Shiny Dust'ed Energy Tesseract receiving Buildcraft power in slot 16 (I suggest using a Redstone Energy Cell on the other side of this Tesseract. This way you can just increase the delay between mining operations to adapt to your energy supply).
* mining: Mining Turtle
  This turtle places the Mining Well and a ender chest (or similar) on the command turtle's, well, command. You can have as many of these as you want (and are able to power, obviously).

  Inventory setup: Some form of fuel source for the turtle in slot 13 (I use a second ender chest for this purpose), 1 Redstone Energy Conduit in slot 14, 1 Ender Chest (or similar) in slot 15, and finally 1 Mining Well in slot 16.
* chunkloader: Mining Turtle
  This turtle is responsible for babysitting the chunkloader. 
  
  You can use chunkloader turtles for this if you manage to squeeze your operation into one chunk, however, that falls flat for larger sizes as there seem to be issues with loading more than one chunk, even if the Computer Craft configuration would allow this.

  Inventory setup: Fuel souce (ender chest or similar) in slot 15, a chunkloader in slot 16.
* energy (optional): Wrench turtle
  This turtle comes into play if the number of your miners rises above 10 or so, or you just feel that the "quarry" isn't running fast enough. It's basically a clone of the command turtle, except that it listens to commands instead of sending them.

  Inventory setup is the same as the command turtle.

Button api
----------

This is an api for ComputerCraft's touch monitors. It's inspired by Direwolf20's button api, but based on what qualifies as class in lua.

Usage:
```os.loadAPI("button") -- load button api
callback = function(button) 
  print("Button '" .. button.text .. "' clicked!") 
  button:flash()
end
b = button.new("Click me!", callback, xMin, xMax, yMin, yMax)

while true do
  button.awaitClick() -- blocks
end```

### API functions
Function name | Description
--------------|------------
`awaitClick()` | Waits for a `monitor_touch` event and calls all applicable button callbacks (should only be one unless you overlap your buttons). You probably want to call this inside a loop.
`new(text, callback, xMin, xMax, yMin, yMax, colors)` | Creates a new button. Colors is an optional table with keys `text`, `background`, `enabled` and `disabled`. They are all optional and default to `colors.white`, `colors.black`, `colors.lime` and `colors.red`, respectively.
`remove(button)` | Removes the button from the internal button list, thus rendering it unclickable. Not sure if this is useful unless you create tons of temporary buttons (consider reusing them).
`setMonitor(monitor)` | Set the (default) monitor explicitely. This is really only useful if you have more than one monitor attached to your computer, because the auto-detection will do this for you otherwise. Note: This is a global setting. This API does not support more than one monitor (unless you implement your own version of `awaitClick` and set the monitor on each button manually).

### Button methods
Method name | Description
------------|------------
`disable()` | Disables a button. Default is enabled.
`display()` | Display the button on screen.
`enable()` | Enables a button. Default is enabled.
`flash(interval)` | Disables the button, waits for the interval, and enables it again. The interval argument is optional and defaults to 0.15s.
`hide()` | Hide the button. Default is visible.
`show()` | Show the button. Default is visible.

### Button attributes
Attribute | Description
----------|------------
callback | Callback function, gets called with the button as argument when the button gets clicked.
colors | Table with keys `background`, `disabled`, `enabled`, `text`. See the colors API for valid values.
enabled | True if the button is enabled, or clickable.
monitor | Monitor the button is rendered to. See `setMonitor` above for remarks about multi-monitor use.
text | Button text.
visible | True if the button is visible.
x | Table with min/max values on the x axis (horizontal)
y | Table with min/max values on the y axis (vertical)


