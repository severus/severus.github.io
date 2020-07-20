---
layout: default
title: Keyboard layout switch shortcut
parent: openSUSE
grand_parent: Linux
nav_order: 1
---

# How to change the default keyboard layout switch shortcut system-wide on openSUSE

With openSUSE's awesome YaST it's possible to set system keyboard layout, but it doesn't allow to set the keyboard layout switch shortcut.
{: .fs-6 .fw-300 }

To set the system keyboard layout switch shortcut on openSUSE do the following.

Fisrt of all, let's set the keyboard layout using YaST:

1. [Start the YaST tool](https://en.opensuse.org/SDB:Starting_YaST)
2. Select *Hardware* > *System Keyboard Layout*
3. Choose the appropriate keyboard layout (for example *Russian*)
4. Accept changes.

Let's see the current keyboard configuration:

```
$ localectl status
   System Locale: LANG=en_US.UTF-8
       VC Keymap: ruwin_alt-UTF-8
      X11 Layout: us,ru
       X11 Model: pc105
     X11 Variant: ,winkeys
     X11 Options: terminate:ctrl_alt_bksp,grp:ctrl_shift_toggle,grp_led:scroll
```

The X11 oprion `grp:ctrl_shift_toggle` means that the keyboard switch shortcut is <kbd>Ctrl+Shift</kbd>. Let's change it to <kbd>Caps Lock</kbd>.

To change keyboard options (for both X11 and virtual console) we'll use `localectl` command:

```
# localectl set-x11-keymap LAYOUT MODEL VARIANT OPTIONS
```

Where `LAYOUT`, `MODEL`, and `VARIANT` is the `X11 Layout`, `X11 Model`, and `X11 Variant` value from the `localectl status` output accordingly. For `OPTIONS` value use the `X11 Options` value from the `localectl status` output, but changing the switch shortcut (the one starting with `grp:`, for example `grp:ctrl_shift_toggle`) with the preferred one, for example `grp:caps_toggle`. For the list of available layout switch shortcuts consult the `OPTIONS` section of the `man 7 xkeyboard-config` output:

| Option                    | Description                                                                |
|:--------------------------|:---------------------------------------------------------------------------|
| grp:switch                | Right Alt (while pressed)                                                  |
| grp:rwin_switch           | Right Win (while pressed)                                                  |
| grp:win_switch            | Any Win (while pressed)                                                    |
| grp:menu_switch           | Menu (while pressed), Shift+Menu for Menu                                  |
| grp:caps_switch           | Caps Lock (while pressed), Alt+Caps Lock for the original Caps Lock action |
| grp:rctrl_switch          | Right Ctrl (while pressed)                                                 |
| grp:toggle                | Right Alt                                                                  |
| grp:lalt_toggle           | Left Alt                                                                   |
| grp:caps_toggle           | Caps Lock                                                                  |
| grp:shift_caps_toggle     | Shift+Caps Lock                                                            |
| grp:shift_caps_switch     | Caps Lock to first layout; Shift+Caps Lock to last layout                  |
| grp:win_menu_switch       | Left Win to first layout; Right Win/Menu to last layout                    |
| grp:lctrl_rctrl_switch    | Left Ctrl to first layout; Right Ctrl to last layout                       |
| grp:alt_caps_toggle       | Alt+Caps Lock                                                              |
| grp:shifts_toggle         | Both Shift together                                                        |
| grp:alts_toggle           | Both Alt together                                                          |
| grp:ctrls_toggle          | Both Ctrl together                                                         |
| grp:ctrl_shift_toggle     | Ctrl+Shift                                                                 |
| grp:lctrl_lshift_toggle   | Left Ctrl+Left Shift                                                       |
| grp:rctrl_rshift_toggle   | Right Ctrl+Right Shift                                                     |
| grp:ctrl_alt_toggle       | Alt+Ctrl                                                                   |
| grp:alt_shift_toggle      | Alt+Shift                                                                  |
| grp:lalt_lshift_toggle    | Left Alt+Left Shift                                                        |
| grp:alt_space_toggle      | Alt+Space                                                                  |
| grp:menu_toggle           | Menu                                                                       |
| grp:lwin_toggle           | Left Win                                                                   |
| grp:win_space_toggle      | Win+Space                                                                  |
| grp:rwin_toggle           | Right Win                                                                  |
| grp:lshift_toggle         | Left Shift                                                                 |
| grp:rshift_toggle         | Right Shift                                                                |
| grp:lctrl_toggle          | Left Ctrl                                                                  |
| grp:rctrl_toggle          | Right Ctrl                                                                 |
| grp:sclk_toggle           | Scroll Lock                                                                |
| grp:lctrl_lwin_rctrl_menu | Left Ctrl+Left Win to first layout; Right Ctrl+Menu to second layout       |
| grp:lctrl_lwin_toggle     | Left Ctrl+Left Win                                                         |

Restart your X11 server (re-login or reboot), enjoy your newly configured keyboard layout switch shortcut!
