Chord logger

Important project settings:

Settings
View
Clock format 1 1  1

Logic scripter chord/time detect:

// Enable timing info var NeedsTimingInfo = true;

var chordNotes = []; var chordStartBeat = 0; var progression = []; // holds only chord names var TICKS_WAIT = 60; // detection wait ticks var PPQ = 960; // pulses per quarter note var OFFSET_TICKS = 942; // calibrate to shift reported position back to actual onset var ticksSinceLastNote = 0;

var PluginParameters = [ { name: "Detected Chord", type: "menu", defaultValue: 0, valueStrings: ["No chord"] }, { name: "Show Full Progression", type: "momentary" }, { name: "Reset Progression", type: "momentary" } ];

// Build chord name list var roots = ["C","Db","D","Eb","E","F","Gb","G","Ab","A","Bb","B"]; var extensions = ["","m","7","maj7","m7","dim7","m7b5","aug","6","m6","9","m9", "maj9","maj13","6/9","m6/9","sus2","sus4","7(sus4)","7(#5)","7(b5)", "7(b9)","7(#9)","7(#9 13)","maj add 9","m11","m11(b9)","13","13(#9)", "13(#11)","maj9(#11)","maj9(#11#5)","maj9(sus4)","maj7(#5)","7(b5 b9)","m(maj7#5)" ]; var knownChords = ["No chord"]; roots.forEach(r => extensions.forEach(e => knownChords.push(r + e))); PluginParameters[0].valueStrings = knownChords;

function updateUI(name) { var idx = knownChords.indexOf(name); SetParameter(0, idx !== -1 ? idx : 0); }

function HandleMIDI(event) { if (event instanceof NoteOn) { // record chord onset beat on first note if (chordNotes.length === 0) { chordStartBeat = GetTimingInfo().blockStartBeat; } if (!chordNotes.includes(event.pitch)) chordNotes.push(event.pitch); ticksSinceLastNote = 0; } event.send(); }

function ProcessMIDI() { if (chordNotes.length > 0) { ticksSinceLastNote++; if (ticksSinceLastNote >= TICKS_WAIT) { chordNotes.sort((a, b) => a - b); var name = getChordName(chordNotes);

  // calculate onsetBeat directly
  var onsetBeat = chordStartBeat;
  var info = GetTimingInfo();
  var meter = info.meterNumerator;
  var measure = Math.floor(onsetBeat / meter) + 1;
  var fracBeat = onsetBeat % meter;
  var beatNum = Math.floor(fracBeat) + 1;
  var ticksRaw = Math.round((fracBeat - Math.floor(fracBeat)) * PPQ);
  // apply offset
  var ticksAdj = ticksRaw - OFFSET_TICKS;
  // wrap around if negative
  if (ticksAdj < 0) {
    ticksAdj += PPQ;
    beatNum -= 1;
    if (beatNum < 1) {
      measure -= 1;
      beatNum = meter;
    }
  }
  var projectPos = measure + ":" + beatNum + ":" + ticksAdj;

  Trace(projectPos + "," + name);
  progression.push(name);
  updateUI(name);

  chordNotes = [];
  ticksSinceLastNote = 0;
}
} }

function ParameterChanged(param, value) { if (param === 1) Trace("\nFull Progression:\n" + progression.join(", ")); if (param === 2) { progression = []; Trace("\nProgression reset."); } }

function getNoteName(pitch) { var names = ["C","Db","D","Eb","E","F","Gb","G","Ab","A","Bb","B"]; return names[pitch % 12]; }

// Extended chord library (unchanged) var chordLibrary = [ { name: "m(maj7#5)", intervals: [0,3,8,11] }, { name: "m11(b9)", intervals: [0,3,7,10,1,2,5] }, { name: "m11", intervals: [0,3,7,10,2,5] }, { name: "7(b5 b9)", intervals: [0,4,6,10,1] }, { name: "13(#9)", intervals: [0,4,7,10,3,9] }, { name: "7(#9 13)", intervals: [0,4,7,10,3,9] }, { name: "maj9(#11)", intervals: [0,4,7,11,2,6] }, { name: "maj7(#5)", intervals: [0,4,8,11] }, { name: "maj9(sus4)", intervals: [0,5,7,2,4] }, { name: "m9", intervals: [0,3,7,10,2] }, { name: "9", intervals: [0,4,7,10,2] }, { name: "m7", intervals: [0,3,7,10] }, { name: "7", intervals: [0,4,7,10] }, { name: "maj7", intervals: [0,4,7,11] }, { name: "m7b5", intervals: [0,3,6,10] }, { name: "dim7", intervals: [0,3,6,9] }, { name: "7(#5)", intervals: [0,4,8,10] }, { name: "m6/9", intervals: [0,3,7,9,2] }, { name: "6/9", intervals: [0,4,7,9,2] }, { name: "m", intervals: [0,3,7] }, { name: "", intervals: [0,4,7] }, { name: "sus4", intervals: [0,5,7] }, { name: "sus2", intervals: [0,2,7] }, { name: "7(sus4)", intervals: [0,5,10] } ];

function getChordName(pitches) { if (!pitches.length) return "No chord"; var best = { name: "?", root: pitches[0], match: 0, weight: 0 }; for (var i = 0; i < pitches.length; i++) { var root = pitches[i]; var intervals = pitches.map(p => (p - root + 12) % 12); var unique = Array.from(new Set(intervals)); chordLibrary.forEach(function(ch) { var matchCount = ch.intervals.filter(iv => unique.includes(iv)).length; var ratio = matchCount / ch.intervals.length; var isDomForm = ch.name.includes("7") && !ch.name.includes("maj"); if (ratio >= 0.75 && matchCount > best.match) { best = { name: ch.name, root: root, match: matchCount, weight: ratio + (isDomForm && unique.includes(4) && unique.includes(10) ? 0.1 : 0) }; } }); } var rootName = getNoteName(best.root); var bass = getNoteName(pitches[0]); var chordName = rootName + best.name; if (best.root % 12 !== pitches[0] % 12) chordName += " / " + bass; return chordName; }

Apple Shortcut, automate marker positions and chord name input:

Command option control space —> chord marker place

on run -- 1) Read all lines from the clipboard set lineList to paragraphs of (the clipboard)

tell application "System Events"
	repeat with i from 1 to count of lineList
		set thisLine to item i of lineList
		if thisLine is not "" then
			-- 2) Split into Bars:Beats:Ticks and chord name
			set AppleScript's text item delimiters to ","
			set {posString, chordNameRaw} to text items of thisLine
			set AppleScript's text item delimiters to ""
			
			-- 3) Clean up chord name
			set cleanedChord to my trimText(chordNameRaw)
			
			-- A) Go to Position
			keystroke "z"
			delay 0.15
			-- Paste exact time string
			set the clipboard to posString
			delay 0.1
			keystroke "v" using {command down}
			delay 0.15
			key code 36 -- Return
			delay 0.15
			
			-- B) Create Marker (Control+Option+M)
			key code 46 using {control down, option down}
			delay 0.15
			
			-- C) Rename Marker with '=' key
			key code 24 -- '='
			delay 0.1
			-- Type the cleaned chord name
			keystroke cleanedChord
			delay 0.15
			key code 36 -- Return
			delay 0.15
		end if
	end repeat
end tell
end run

-- Helper: trim whitespace on trimText(txt) set txt to txt as string set txt to do shell script "echo " & quoted form of txt & " | sed 's/^ *//;s/ *$//'" return txt end trimText

Instructions:

Open a software instrument track and select Scripter in the MIDI FX slot.

Replace the existing script with the provided code.

Enable timing info by setting var NeedsTimingInfo = true and ensuring the plugin settings have "Receive Timing" enabled.

Save the preset as "Chord Logger" or similar. For the Apple Shortcut setup:

Use the clipboard to copy the plugin’s console output of detected chord data and run the shortcut. Detailed setup instructions for Logic Pro and Apple Shortcut

In Logic Pro, map the following key commands:

"z" to "Go to Position" (Transport > Go to Position).
Control + Option + M to "Create Marker".
"=" to "Rename Marker".
In Logic's Key Commands, cancel the default assignment for Control + Option + Command + Space to allow the Apple Shortcut to trigger.

In the Shortcuts app, create a new shortcut named "Chord logger".

Add the "Run AppleScript" action and paste the script inside.
Set Command + Option + Control + Space as the shortcut trigger. Thought for 12 seconds Here’s a step-by-step guide to get everything running smoothly. We’ll cover:
Logic Pro key-command setup

Installing the Scripter plugin

Assigning the shortcut in macOS Shortcuts

Disabling the macOS “⌃⌥⌘Space” conflict

Workflow: logging → copying → marker creation

Logic Pro key-command setup

Open Logic Pro → Key Commands → Edit…

Map “Go to Position…” to z

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

Delete all default code and paste in the full “Logic scripter chord/time detect” script you provided.

At the top of the plugin window make sure Enable is on.

Click the Preset dropdown → Save As… → name it Chord/Time Detect.

Close the editor. Tip: you don’t need to touch the plugin’s UI—your script’s NeedsTimingInfo = true already grabs beat positions.

Create the macOS Shortcut

Open Shortcuts.app.

Click + to make a new shortcut, name it Create Chord Markers.

Add action Run AppleScript, paste your Apple Shortcut code block.

Click the ⚙️ (settings) button in the upper right.

Toggle Use as Quick Action if you like.
In Keyboard Shortcut, click none then press ⌃⌥⌘Space.
Close the settings, then File → Save.

Disable the macOS “⌃⌥⌘Space” conflict By default, macOS uses Control + Command + Space (with or without Option) to open the Emoji & Symbols panel. To free it:

Open System Settings → Keyboard → Keyboard Shortcuts.

Go to Input Sources (or “Show Emoji & Symbols”).

Uncheck the shortcut for Show Emoji & Symbols. Now ⌃⌥⌘Space will reliably fire your “Create Chord Markers” shortcut instead.

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
