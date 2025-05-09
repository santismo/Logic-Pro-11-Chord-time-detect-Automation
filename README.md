Chord logger

Important project settings:

Settings
View
Clock format 1 1  1

Also, map the following key commands:

Map “Go to Position…” to z

Map “Create Marker without rounding” to Control + Option + M

Map “Rename Marker” to =

Unassign any Logic command using ⌃⌥⌘Space

Video explanation: 
https://www.youtube.com/shorts/LtUfBbqKwLI

Video Instructions:
https://youtu.be/g8brcsVjSEI?si=Xq0rml-RQuBYNH3X

--------------------------------------
Logic scripter chord/time detect:

Download and copy code from scripter raw code text file into scripter, or save scripter pst file to project if you know how:

--------------------------------------

Apple Shortcut, automate marker positions and chord name input:

Command option control space —> chord marker place

Download and copy raw code from applescript text file into shortcuts, choose desired shortcut command, or download and save shortcut automation file if you know how:

--------------------------------------

--------------------------------------

--------------------------------------

Detailed Instructions for Newbs:



Workflow: logging → copying → marker creation
--------------------------------------

Important project settings:

Settings
View
Clock format 1 1  1
--------------------------------------

Logic Pro key-command setup:

Open Logic Pro → Key Commands → Edit…

Map “Go to Position…” to z
Search for Transport → Go to Position…
Click Learn by Key Label, press z, click Assign.

Map “Create Marker without rounding” to Control + Option + M
Search for Global Tracks → Marker → Create Marker without rounding
Click Learn by Key Label, press ⌃⌥M, click Assign.

Map “Rename Marker” to =
Search for Global Tracks → Marker → Rename Marker
Click Learn by Key Label, press =, click Assign.

Unassign any Logic command using ⌃⌥⌘Space
In the same window, search by pressing Learn by Key Label, then hit ⌃⌥⌘Space
If it finds a command, select it and click Clear.

Click Save and give your key-commands set a name (e.g. “ChordLogger”).


--------------------------------------

Install & configure the Scripter plugin:

In your project, create (or select) a Software Instrument track.

In the MIDI FX slot, choose Scripter → Open Editor.

Delete all default code and paste in the full “Logic scripter chord/time detect” script.

At the top of the plugin window make sure Enable is on.

Click the Preset dropdown → Save As… → name it Chord/Time Detect.

Close the editor. Tip: you don’t need to touch the plugin’s UI—your script’s NeedsTimingInfo = true already grabs beat positions.


--------------------------------------

Create the macOS Shortcut

Open Shortcuts.app.

Click + to make a new shortcut, name it Create Chord Markers.

Add action Run AppleScript, paste Apple Shortcut code block.

Click the ⚙️ (settings) button in the upper right.

Toggle Use as Quick Action if you like.
In Keyboard Shortcut, click none then press ⌃⌥⌘Space.
Close the settings, then File → Save.


--------------------------------------
Optional*

Disable the macOS “⌃⌥⌘Space” conflict By default, macOS uses Control + Command + Space (with or without Option) to open the Emoji & Symbols panel. To free it:

Open System Settings → Keyboard → Keyboard Shortcuts.

Go to Input Sources (or “Show Emoji & Symbols”).

Uncheck the shortcut for Show Emoji & Symbols. Now ⌃⌥⌘Space will reliably fire your “Create Chord Markers” shortcut instead.


--------------------------------------
--------------------------------------
--------------------------------------

Full workflow

Play or record your MIDI track with the Scripter plugin active.

Open the Scripter Console (click the little “Trace” icon in the plugin).

When you’re done, click inside that console window, select chord and time output, the  ⌘C (Copy).

Switch back to the Arrange window.

Press ⌃⌥⌘Space.

The shortcut will parse each “bar:beat:tick,chordName” line,
Jump to that position (z → paste → Return),
Create a marker (⌃⌥M),
Press = to rename,
Type the chord name and hit Return. And you’re done—Logic Pro will place global markers at every detected chord, named accordingly.
