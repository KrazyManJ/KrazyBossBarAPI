# KrazyBossBarAPI

## About

This skript API (addon) can manipulate with bossbars. It can create, modify, or remove bossbars.

**Those bossbars are server side, so if you need to make client side bossbars, use packets instead of this addon!** *Mby in future i will add them there!*

### Requirements:
- [Skript](https://github.com/SkriptLang/Skript/releases)
- [skript-reflect](https://github.com/TPGamesNL/skript-reflect/releases)

### Features:
- [x] Creating bossbar
- [x] Removing bossbar
- [x] Changing title of bossbar
- [x] Changing progress/value/damage of bossbar
- [x] Changing color of bossbar
- [x] Changing style of bossbar
- [x] Adding/removing flags of bossbar
- [x] Adding/removing players seing specific bossbar
- [x] Hiding/Showing bossbar globally
- [ ] Force-changing bossbar progress *(COMMING REALLY SOON)*


## Syntax

### Utility expressions

##### Colors:
```applescript
set {_colors::*} to all boss bar colors
```
##### Styles:
```applescript
set {_styles::*} to all boss bar styles
```
##### Flags:
```applescript
set {_flags::*} to all boss bar flags
```

### Expressions

#### Creating bossbar
This expression will create new bossbar and save it to variable to be accessed with all expressions down bellow.
You have to set all parameters of boss bar except flags, because they are in most cases irrelevant.

##### arguments:
- `title` - exactly what it says, specifically shown text above bossbar (you can use colors, and also hex colors by formatting `<##afAF09>`)
- `progress` - value of bossbar, also called damage, must be value between `0` and `1` *(in java its called `double` value)*
- `color` - color of bossbar, you can only choose from: 
	- `"BLUE"`
	- `"GREEN"`
	- `"PINK"`
	- `"PURPLE"`
	- `"RED"`
	- `"WHITE"`
	- `"YELLOW"`
- `style` - style of bossbar, you can choose from:
	- `"SOLID"`
	- `"SEGMENTED_6"`
	- `"SEGMENTED_10"`
	- `"SEGMENTED_12"`
	- `"SEGMENTED_20"`
- `flags` *(OPTIONAL)* - you can set if bossbar will create fog, play boss music or make sky little bit darker, all flags:
	- `"CREATE_FOG"`
	- `"DARKEN_SKY"`
	- `"PLAY_BOSS_MUSIC"`
- `players` - players, for that this bossbar will be shown
- `hiding old one` - you can add this to the end of creating to hide old one with same key. If you don't use it, old bossbar will just stay on screen, until player disconnets.

##### Example:
```applescript
set {_bossbar} to new boss bar with key "welcome-bossbar", title "&aWelcome to our server!", progress 0.25, color "GREEN", style "SOLID", flags ("CREATE_FOG","DARKEN_SKY") for all players with hiding old one
```

#### Getting already created bossbar

This expression will find already created bossbar by its key (ID). Usefull when creating bossbar in one event and then use it in another.

##### Example:
```applescript
set {_bossbar} to bossbar with key "welcome-bossbar"
```

#### Getting/changing/reseting title off bossbar:

##### Example:
```applescript
set {_bossbar} to bossbar with key "welcome-bossbar"
set {_title} to title of bossbar {_bossbar}
set title of bossbar {_bossbar} to "I hope you like this server!"
remove title of bossbar {_bossbar} #Value = &r
```

Editing color:
#### Example:
```applescript
set {_bossbar} to bossbar with key "welcome-bossbar"
set {_color} to color of bossbar {_bossbar} 
set color of bossbar {_bossbar} to "YELLOW"
set color of bossbar {_bossbar} to random element of all boss bar colors
```

Editing Style:
#### Example:
```applescript
set {_bossbar} to bossbar with key "welcome-bossbar"
set {_style} to style of bossbar {_bossbar} 
set style of bossbar {_bossbar} to "YELLOW"
set color of bossbar {_bossbar} to random element of all boss bar styles
```

Adding/Removing flags:
#### Example:
```applescript
set {_bossbar} to bossbar with key "welcome-bossbar"
set {_flags::*} to flags of bossbar {_bossbar}
add "PLAY_BOSS_MUSIC" to flags of bossbar {_bossbar} 
remove "CREATE_FOG" from flags of bossbar {_bossbar}
```

Adding/Removing players seeing bossbar:
#### Example:
```applescript
set {_bossbar} to bossbar with key "welcome-bossbar"
add event-player to all players seeing bossbar {_bossbar} 
remove target player from all players seeing bossbar {_bossbar}
reset all players seeing bossbar {_bossbar}
```

Set visibility of bossbar:#### Example:
```applescript
set {_bossbar} to bossbar with key "welcome-bossbar"
set {_cansee} to visibility of bossbar {_bossbar} 
set visibility of bossbar {_bossbar} to false
```

## Conditions

If bossbar is visible:#### Example:
```applescript
set {_bossbar} to bossbar with key "welcome-bossbar"
if boss bar {_bossbar} is visible:
	set visibility of bossbar {_bossbar} to false
```

## Effects

Remove bossbar with key:
#### Example:
```applescript
remove boss bar with key "welcome-bossbar"
```

## Examples:

#### Make bossbar to show message for certain seconds *(progress bar is showing duration)*

##### Example:
```applescript
every 30 seconds:
	set {_bossbar} to new boss bar with key "welcome-bossbar", title "<##34a4eb>&lWelcome to our server!", progress 0, color "BLUE", style "SOLID" for all players with hiding old one
	set {_sec} to 5 # Amount of seconds to appear
	set {_d} to 1/({_sec}*20)
	loop ({_sec}*20) times:
		set progress of bossbar {_bossbar} to (loop-value)*{_d}
		wait 1 tick
	remove bossbar with key "welcome-bossbar"
```

#### Make bossbar to follow hp status of entity *(in this case i am using sheep)*

##### Example:
```applescript
on spawn of sheep:
	set {_uuid} to random uuid # Ability to have multiple sheep
	set {_names::*} to "Merlin","Tobby","Krazy","Milo","Bobby" # Random names :3
	set {_bossbar} to new boss bar with key "sheep_%{_uuid}%", title "&lMAGIC SHEEP - %random element of {_names::*} in uppercase%", progress 1, color "WHITE", style "SOLID" for all players with hiding old one
	set {_max} to max health of event-entity
	set {_h} to health of event-entity
	while event-entity is alive:
		set progress of bossbar {_bossbar} to (1/{_max})*{_h}
		set {_h} to health of event-entity
		wait 1 tick
	set progress of bossbar {_bossbar} to 0 #If i instakill it, it will stay to 1 so i will set it to 0 there
	wait 1 second #It takes 1 second to mob to disapear after death
	remove bossbar by key "sheep_%{_uuid}%"
```

#### MAKE COUNTDOWN COMMAND AND SHOW TIMER THROUGH BOSSBAR

##### Example:
```applescript
on load:
	set {-bossbar} to new boss bar with key "countdown", title "<##ffff00>&lEVENT IS ENDING IN: xs", progress 1, color "YELLOW", style "SOLID" for all players with hiding old one
	set visibility of boss bar {-bossbar} to false

command /countdown <integer>:
	trigger:
		set {_d} to 1/(arg-1)
		set visibility of boss bar {-bossbar} to true
		loop arg-1 times:
			set {_time} to (arg-1)-((loop-value)-1)
			if {_time} <= 3:
				set color of boss bar {-bossbar} to "RED"
				set title of boss bar {-bossbar} to "<##ff0000>&lEVENT IS ENDING IN: %{_time}%s"
				set progress of boss bar {-bossbar} to 1-({_d}*((loop-value)-1))
				wait 0.5 second
				stop if "%progress of boss bar {-bossbar}%" is not "%1-({_d}*((loop-value)-1))%"
				set color of boss bar {-bossbar} to "YELLOW"
				set title of boss bar {-bossbar} to "<##ffff00>&lEVENT IS ENDING IN: %{_time}%s"
				wait 0.5 second
				stop if "%progress of boss bar {-bossbar}%" is not "%1-({_d}*((loop-value)-1))%"

			else:
				set title of boss bar {-bossbar} to "<##ffff00>&lEVENT IS ENDING IN: %{_time}%s"
				set progress of boss bar {-bossbar} to 1-({_d}*((loop-value)-1))
				wait 1 second
				stop if "%progress of boss bar {-bossbar}%" is not "%1-({_d}*((loop-value)-1))%"
		set title of boss bar {-bossbar} to "<##ffff00>&lEVENT ENDED!"
		set progress of boss bar {-bossbar} to 0
		wait 1 second
		if progress of boss bar {-bossbar} is 0:
			set visibility of boss bar {-bossbar} to false
```


#### MAKE BOSSBAR TO FADE MESSAGE CHARACTER BY CHARACTER WITH PROGRESS BAR FADING AT SAME SPEED AS SHOWING MESSAGE FOR CERTAIN SECONDS, AND FADE IT BACK AWAY

##### Example:
```applescript
command /bossbar:
	trigger:
		set {_bossbar} to new boss bar with key "show", title "&l", progress 0, color "WHITE", style "SOLID" for all players with hiding old one
		set {_text} to "WELCOME HERE! ENJOY YOUR STAY!"
		set {_chars::*} to split {_text} at ""
		loop (length of {_text}) times:
			wait 1 tick
			set title of boss bar {_bossbar} to join (title of boss bar {_bossbar}),{_chars::%loop-value%}
			set progress of boss bar {_bossbar} to 1/(length of {_text})*(loop-value)
		wait 5 second
		loop (length of {_text}) times:
			wait 1 tick
			set {_title} to title of boss bar {_bossbar}
			set {_tchars::*} to split {_title} at ""
			set {_return} to ""
			loop (length of {_title})-1 times:
				set {_return} to join {_return},{_tchars::%loop-value-2%}
			set title of boss bar {_bossbar} to join "&l",{_return}
			set progress of boss bar {_bossbar} to 1-(1/(length of {_text})*(loop-value))
		remove bossbar by key "show"
```