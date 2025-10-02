```C
#define scrUtilityDraw
///scrUtilityDraw(DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL)
// this is an empty script for the sake of the nested scripts
return undefined;

#define draw_text_outline_x
///draw_text_outline(xx, yy, str, col, off);
var xx = argument0;
var yy = argument1;
var str = argument2;
var col = argument3;
var off = argument4;

var c = draw_get_color()
draw_set_color(col)
draw_text(xx + off, yy + off, str)
draw_text(xx - off, yy + off, str)
draw_text(xx - off, yy - off, str)
draw_text(xx + off, yy - off, str)
draw_set_color(c)
draw_text(xx, yy, str)

#define draw_9slice_stretch
///scrP2Draw9Slice_Stretch(spr, x, y, w, h)
#args spr, xx, yy, w, h
var
_sw = sprite_get_width(spr),
_sh = sprite_get_height(spr),
_middle_xscl = (w - _sw * 2) / _sw,
_middle_yscl = (h - _sh * 2) / _sh;

draw_sprite(spr, 0, xx, yy);
draw_sprite_ext(spr, 1, xx + _sw, yy, _middle_xscl, 1, 0, c_white, 1.0);
draw_sprite(spr, 2, xx + w - _sw, yy);

draw_sprite_ext(spr, 3, xx, yy + _sh, 1, _middle_yscl, 0, c_white, 1.0);
draw_sprite_ext(spr, 4, xx + _sw, yy + _sh, _middle_xscl, _middle_yscl, 0, c_white, 1.0);
draw_sprite_ext(spr, 5, xx + w - _sw, yy + _sh, 1, _middle_yscl, 0, c_white, 1.0);

draw_sprite(spr, 6, xx, yy + h - _sh);
draw_sprite_ext(spr, 7, xx + _sw, yy + h - _sh, _middle_xscl, 1, 0, c_white, 1.0);
draw_sprite(spr, 8, xx + w - _sw, yy + h - _sh);
```