# Config file structure
Wayfire is configured through a simple ini file, which is located under `$HOME/.config/wayfire.ini`
It is divided into sections, which have the following format:

    [<plugin name>]
    option_name1 = value1
    option_name2 = value2
    ....

The only "special" section is the core section. Refer to it's description below for a better understanding of it.
There are several types of values:
Int, double, string

Color - a series of four floating point numbers representing a color in RGBA format

Button - represents a combination of modifier + button. Modifiers can be `<shift>` `<ctrl>` `<alt>` `<super>`. Buttons can be: `left` `right` `middle`

Key- represents a combination + modifier + key. Modifiers are the same as buttons, key is in the format specified in `<linux/input-event-codes.h>`. Most probably, if you want some character from the alphabet, the format is KEY_"your char", for ex. `KEY_E`. For more examples see the sample config file.

# Example
     [core]                                                                                                       
     plugins = viewport_impl move resize command vswitch switcher grid expo animate oswitch backlight rotator cube
     vwidth = 3
     vheight = 3
     xkb_layout = us,bg
     xkb_variant = ,phonetic
     xkb_option = grp:win_space_toggle
       
     [shell]
     background = /home/ilex/Downloads/phoenix6.png
     color_temp_enabled = 1
     day_start = 06:30
     day_end = 20:00
     day_temperature = 5700
     night_temperature = 4500
         
     [command]
     command_1 = bemenu-run
     binding_1 = <super> KEY_D
     command_2 = terminator
     binding_2 = <super> KEY_T
     command_3 = amixer -q sset Master 5%+
     binding_3 = KEY_VOLUMEUP
     command_4 = amixer -q sset Master 5%-
     binding_4 = KEY_VOLUMEDOWN
         
     [vswitch]
     binding_left = <super> KEY_DOWN
     binding_right = <super> KEY_KP0
     binding_up = <super> KEY_RIGHTSHIFT
     binding_down = <super> KEY_RIGHT
     duration = 300
     
     [grid]
     duration = 180
         
     [move]
     enable_snap = 1
       
     [switcher]
     duration = 180
       
     [backlight]
     backend = intel
         
     [expo]
     duration = 300
     offset = 0
         
     [rotator]
     rotate_up = <ctrl> <alt> KEY_RIGHTSHIFT
     rotate_down = <ctrl> <alt> KEY_RIGHT
     rotate_left = <ctrl> <alt> KEY_DOWN
     rotate_right = <ctrl> <alt> KEY_KP0
         
     [cube]
     light = 1
     deform = 1


# Currently available sections/options
## Core

This is the most important section in the config file. It has options related to the base configuration.

### Options

`plugins`

A list of space separated plugins that should be loaded. By default nothing is loaded

`vwidth/vheight`

By default the workspaces are arranged in a grid. This indicates the grid's width/height
Default values: 3 3

`xkb_layout/xkb_variant/xkb_option/xkb_model/xkb_rule`

These describe the corresponding xkb options, they follow the same format which is used for setxkbmap
Default is empty

`kb_repeat_rate/kb_repeat_delay`

Control the behavior of continuous keypress.
Default: 40/400

`run_panel`

Whether to start built-in desktop client(provides background and the panel at the top)
Default: 1

`shadersrc/plugin_path_prefix`
You normally shouldn't touch these. They indicate the path to shaders used for rendering in GLES and the path to plugins.

## shell
This contains the configuration for the built-in shell client.

### Options
`background`

The path to background, which must be in png format

`color_temp_enabled`

Enable/disable gamma adjustment according to the time of day. Similar to redshift/f.lux
Default: 1

`day_start/day_end`

Time in hh:mm format, the times between which the day color temperature should be used.
Default: day_start = 06:30 day_end = 20:00

`day_temperature/night_temperature`

Screen color temperature in kelvins for the corresponding period of day
Default: 6500, 4500

# Plugins options
## command

This plugin allows to bind specific keys to certain commands. Useful for starting a terminal/controlling sound via alsamixer/etc.
Syntax is:

    command_n = <command>
    binding_n = <keybinding>

Where n is some natural number.

## vswitch

Provides keybindings to switch the current workspace to the one on the left/right/top/bottom in the workspace grid. 

options:

`binding_left/right/up/down`

Keybinding for the corresponding direction

`duration`

Duration, in milliseconds, of the animation.

## grid

Provides basic tiling support. With it you can "tile" windows in one of the four corners of the screen or in the left/right/top/bottom half, and also maximize/unmaximize them.
Important: this plugin is required for adjusting window size when it is maximized/fullscreened, otherwise those won't work.

Options:

`slot_{bl, br, tl, tr, t, b, r, l, c}`

Keybinding to move currently focused window to the corresponding slot. b - bottom, l - left, etc. bl means combination of bottom and left(quarter of the screen), c - center the window, i.e maximize it
Default values:
Ctrl + Alt + keypad numbers, there for example 7(which is at the top-left) corresponds to the tl slot.

 `duration`

Animation duration in msecs.
