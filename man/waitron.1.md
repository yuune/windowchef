waitron(1) -- A client for windowchef(1)
==========================================

## SYNOPSIS

`waitron` [-hv] <command> [<args>...]

## DESCRIPTION

`waitron` is the client for windowchef(1). It sends a given command to
windowchef(1) as an X client message. As a result, `waitron` doesn't print
anything on `stdout`.

## OPTIONS

* `-h`:
	Print usage.

* `-v`:
	Print version information.

## COMMON DEFINITIONS

* `POSITION`:
	topleft | topright | bottomleft | bottomright | middle

* `BOOL`:
	true values: `true` | `t` | `yes` | `y` | `1`

	false values: `false` | `f` | `no` | `n` | `0`

* `DIRECTION`:
	`up` | `down` | `left` | `right` | `north` | `south` | `west` | `east`

* `POINTER_ACTION`:
	`nothing` | `focus` | `move` | `resize_corner` | `resize_side`

* `POINTER_MODIFIER`:
	`alt` | `super`

* `MOUSE_BUTTON`:
	`any` | `none` | `left` | `middle` | `right`

## COMMANDS

* `window_move` <x> <y>:
	Move the focused window <y> pixels horizontally and <y> pixels
	vertically.

* `window_move_absolute` <x> <y>:
	Move the focused window to the position given by <x> and <y>.

* `window_resize` <x> <y>:
	Resize the focused window based on the relative pixel values <x> and <y>.

* `window_resize_absolute` <width> <height>:
	Resize the focused window up to <width> and <height>.

* `window_maximize`:
	Hide the border and maximize the focused window on the current monitor.
	Execute it again after maximizing to revert the state of the window.

* `window_unmaximize`:
	If the currently focused window is maximized in any way (fully, vertically, horizontally, monocled), return it to normal state.

* `window_hor_maximize`:
	Horizontally maximize the focused window on the current monitor, preserving
	its <x> component. Leaves a gap at the left and right of the monitor that
	can be configured. Execute it again after maximizing to revert the state of the window.

* `window_ver_maximize`:
	Vertically maximize the focused window on the current monitor, preserving
	its <y> component. Leaves a gap at the top and bottom of the monitor whose
	width can be configured. Execute it again after maximizing to revert the state of the window.

* `window_monocle`:
	Puts the window in the "monocled" state: full screen but with borders
	visible. Monocle mode respects gaps.

* `window_close`:
	Closes the focused window.

* `window_put_in_grid` <grid_width> <grid_height> <cell_x> <cell_y>:
	Moves and resizes the focused windows accordingly to fit in a cell defined
	by the <cell_x> and <cell_y> coordinates in a virtual grid with width
	<grid_width> and height <grid_height> on the current monitor.
	Gaps around the windows in the grid can be added along with monitor gaps.

* `window_snap` <POSITION>:
	Snap the window on the screen in a position defined by <POSITION>.

* `window_cycle`:
	Cycle through mapped windows.

* `window_rev_cycle`:
	Reverse cycle through mapped windows.

* `window_cycle_in_group`:
	Cycle trough mapped windows that belong to the same group as the
	focused window.

* `window_rev_cycle_in_group`:
	Reverse cycle through mapped windows that belong to the same group as the
	focused window.

* `window_focus` <id>:
	Focus window by <id>. The <id> can be found using pfw(1) or lsw(1) from
	[wmutils](https://github.com/wmutils/core/).

* `window_focus_last`:
	Focus the window that was focused before the currently focused window.

* `window_cardinal_focus` <DIRECTION>:
	Focus the closest window in a direction, relative to the currently
	currently focused window. Does nothing if there is no window focused.

* `group_add_window` <group_nr>:
	Add the focused window to the <group_nr> group.

	Group numbers start from 0 and end at <GROUPS_NO> - 1.

* `group_remove_window`:
	Remove the focused window from its current group.

* `group_remove_all_windows` <group_nr>:
	Remove all windows from the <group_nr> group.

* `group_activate` <group_nr>:
	Map all windows that belong to the <group_nr> group.

* `group_deactivate` <group_nr>:
	Unmap all windows that belong to the <group_nr> group.

* `group_toggle` <group_nr>:
	Toggle the <group_nr> group.

* `group_activate_specific` <group_nr>:
	Activate group <group_nr> and deactivate the rest.

* `wm_quit` <exit_status>:
	Quit windowchef with exit_status <exit_status>.

* `wm_change_number_of_groups` <number_of_groups>:
	Change the number of maximum groups to <number_of_groups>.

	Windows that belong to a group that has a <group_nr> greater or equal to
	<number_of_groups> will become orphaned.

* `wm_config` <key> [<values>...]:
	See [CONFIGURING][].

## QUERYING

Information about the current state of windowchef is made available through
X properties of the root window. Example:

```
xprop -root WINDOWCHEF_ACTIVE_GROUPS
```

Here is a list of exposed properties:

* `WINDOWCHEF_ACTIVE_GROUPS`:
	An integer list of currently active groups.

## CONFIGURING

Configuring is done using the `wm_config` command. Possible configuration keys
are:

* `border_width` <width>:
	Sets the border width to <width> pixels.

* `color_focused`, `color_unfocused` <color>:
	Sets the border color to <color> for the focused and unfocused state respectively.
	<color> is a hexadecimal value that may or may not start with `0x`
	prefix. Example: `0x1234ef`.

* `internal_border_width` <width>:
	Make the first <width> pixels from the interior to the exterior of the
	border of another color, so you get two borders.
	The width of the external border is `border_width -
	internal_border_width`.

* `internal_color_focused`, `internal_color_unfocused` <color>:
	Like `color_focused` and `color_unfocused`, but for the internal
	border.

* `gap_width` <POSITION> <width>:
	Sets the window gap at <POSITION> to <width>. <POSITION> can be equal to
	`all` to set all gaps to <POSITION>.

* `grid_gap_width` <width>:
	Sets the window gap value used in virtual grids to <width>.

* `cursor_position` <POSITION>:
	Sets the position of the cursor when moving or resizing windows.

* `groups_nr` <nr>:
	Sets the number of groups to <nr>. If <nr> is less than the current number
	of groups, window that belong to groups whose numbers are greater than <nr>
	will be mapped to screen and assigned to the null group.

* `enable_resize_hints` <BOOL>:
	If true, `windowchef` will respect window resize hints as defined by ICCCM. Most terminal emulators should have this feature.

* `enable_sloppy_focus` <BOOL>:
	Enable sloppy focus.

* `sticky_windows` <BOOL>:
	If <sticky_windows> is true, new windows will be assigned to the last
	activated group automatically. Recommended for people who like using
	workspaces over groups.

* `enable_borders` <BOOL>:
	If true, border colors will be set each time a window gets/loses focus.
	Setting it to false is useful when using another program to draw the borders
	(example: `chwb2` from wmutils).

* `enable_last_window_focusing` <BOOL>:
	If true, when the currently focused window is unmapped or closed, `windowchef` will focus the
	previously focused window. See the `window_focus_last` command.

* `apply_settings` <BOOL>:
	If true, then some settings will be applied on all windows instead of newly created windows.
	True by default.

* `replay_click_on_focus` <BOOL>:
	If true, when clicking on an unfocused with the intent to focus it, windowchef will also send
	the click event to the target window. If false, the window will receive the click event only
	if it's already focused.

* `pointer_actions` <POINTER_ACTION> <POINTER_ACTION> <POINTER_ACTION>:
	Sets the action that should be done whenever the modifier key and the corresponding button
	are clicked at the same time on the window. There are 3 actions for three mouse buttons:
	left, middle and right.

* `pointer_modifier` <POINTER_MODIFIER>:
	Set the modifier for pointer actions.

* `click_to_focus` <MOUSE_BUTTON>:
	Set the mouse button that focuses the hovered window when clicked.
## SEE ALSO

windowchef(1), sxhkd(1), wmutils(1), pfw(1), lsw(1), chwb2(1), lemonbar(1)

## REPORTING BUGS

`windowchef` issue tracker: https://github.com/tudurom/windowchef/issues

## AUTHOR

Tudor Roman `<tudurom at gmail dot com>`
