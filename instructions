Chord logger

Important project settings:

Settings
View
Clock format 1 1  1


Video explanation: 
https://www.youtube.com/shorts/LtUfBbqKwLI

Logic scripter chord/time detect:

Copy code from scripter raw code text file into scripter, or save scripter pst file to project if you know how:

--------------------------------------


Apple Shortcut, automate marker positions and chord name input:

Command option control space —> chord marker place

Copy raw code from applescript text file into shortcuts, choose desired shortcut command, or download and save shortcut automation file if you know how:

--------------------------------------



Detailed Instructions for Newbs:

Open a software instrument track and select Scripter in the MIDI FX slot.

Replace the existing script with the scripter code.

Enable timing info by setting var NeedsTimingInfo = true and ensuring the plugin settings have "Receive Timing" enabled.

Save the preset as "Chord Logger" or similar. For the Apple Shortcut setup:

Use the clipboard to copy the plugin’s console output of detected chord data and run the shortcut. Detailed setup instructions for Logic Pro and Apple Shortcut


--------------------------------------
In Logic Pro, map the following key commands:

"z" to "Go to Position" (Transport > Go to Position).
Control + Option + M to "Create Marker".
"=" to "Rename Marker".
In Logic's Key Commands, cancel the default assignment for Control + Option + Command + Space to allow the Apple Shortcut to trigger.

--------------------------------------

In the Shortcuts app, create a new shortcut named "Chord logger".

Add the "Run AppleScript" action and paste the script inside.
Set Command + Option + Control + Space as the shortcut trigger. Thought for 12 seconds Here’s a step-by-step guide to get everything running smoothly. We’ll cover:
Logic Pro key-command setup

--------------------------------------

Installing the Scripter plugin

Assigning the shortcut in macOS Shortcuts

Disabling the macOS “⌃⌥⌘Space” conflict

Workflow: logging → copying → marker creation

Logic Pro key-command setup

Open Logic Pro → Key Commands → Edit…

Map “Go to Position…” to z


--------------------------------------
Search for Transport → Go to Position…
Click Learn by Key Label, press z, click Assign.
Map “Create Marker” to Control + Option + M

Search for Global Tracks → Marker → Create Marker
Click Learn by Key Label, press ⌃⌥M, click Assign.
Map “Edit Marker Text” to =

Search for Global Tracks → Marker → Edit Marker Text
Click Learn by Key Label, press =, click Assign.
Unassign any Logic command using ⌃⌥⌘Space

In the same window, search by pressing Learn by Key Label, then hit ⌃⌥⌘Space
If it finds a command, select it and click Clear.
Click Save and give your key-commands set a name (e.g. “ChordLogger”).

Install & configure the Scripter plugin

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

Add action Run AppleScript, paste your Apple Shortcut code block.

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

When you’re done, click inside that console window, ⌘A (Select All) → ⌘C (Copy).

Switch back to the Arrange window.

Press ⌃⌥⌘Space.

The shortcut will parse each “bar:beat:tick,chordName” line,
Jump to that position (z → paste → Return),
Create a marker (⌃⌥M),
Press = to rename,
Type the chord name and hit Return. And you’re done—Logic Pro will place global markers at every detected chord, named accordingly.
