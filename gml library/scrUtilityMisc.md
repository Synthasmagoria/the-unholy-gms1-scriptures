```C
#define scrUtilityMisc
///scrUtilityMisc(DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL)
// this is an empty script for the sake of the nested scripts
return undefined;

#define print
///print(string)
show_debug_message(argument0);

#define print_instance
/// print_intstance(inst)
#args inst,
var
_varnames = variable_instance_get_names_builtins(inst, false),
_tab = "    ";

print(object_get_name(inst.object_index) + "(" + string(inst.id) + ") {");
for (var i = array_length_1d(_varnames) - 1; i >= 0; i--) {
    print(_tab + _varnames[i] + ": " + string(variable_instance_get(inst, _varnames[i])) + ",");
}
print("}");

#define script_execute_arr
///script_execute_arr(script, ...)
#args script, args
switch (array_length_1d(args)) {
    case 0: return script_execute(script);
    case 1: return script_execute(script, args[0]);
    case 2: return script_execute(script, args[0], args[1]);
    case 3: return script_execute(script, args[0], args[1], args[2]);
    case 4: return script_execute(script, args[0], args[1], args[2], args[3]);
    case 5: return script_execute(script, args[0], args[1], args[2], args[3], args[4]);
    case 6: return script_execute(script, args[0], args[1], args[2], args[3], args[4], args[5]);
    case 7: return script_execute(script, args[0], args[1], args[2], args[3], args[4], args[5], args[6]);
    case 8: return script_execute(script, args[0], args[1], args[2], args[3], args[4], args[5], args[6], args[7]);
    case 9: return script_execute(script, args[0], args[1], args[2], args[3], args[4], args[5], args[6], args[7], args[8]);
    case 10: return script_execute(script, args[0], args[1], args[2], args[3], args[4], args[5], args[6], args[7], args[8], args[9]);
    case 11: return script_execute(script, args[0], args[1], args[2], args[3], args[4], args[5], args[6], args[7], args[8], args[9], args[10]);
    case 12: return script_execute(script, args[0], args[1], args[2], args[3], args[4], args[5], args[6], args[7], args[8], args[9], args[10], args[11]);
    case 13: return script_execute(script, args[0], args[1], args[2], args[3], args[4], args[5], args[6], args[7], args[8], args[9], args[10], args[11], args[12]);
    case 14: return script_execute(script, args[0], args[1], args[2], args[3], args[4], args[5], args[6], args[7], args[8], args[9], args[10], args[11], args[12], args[13]);
    case 15: return script_execute(script, args[0], args[1], args[2], args[3], args[4], args[5], args[6], args[7], args[8], args[9], args[10], args[11], args[12], args[13], args[14]);
    case 16: return script_execute(script, args[0], args[1], args[2], args[3], args[4], args[5], args[6], args[7], args[8], args[9], args[10], args[11], args[12], args[13], args[14], args[15]);
}

#define asset_get_index_type
///asset_get_index_type(asset_name, asset_type)
#args name, type
var _asset = asset_get_index(name);
switch (type)
{
    case asset_object: if (object_exists(_asset)) return _asset; break;
    case asset_sprite: if (sprite_exists(_asset)) return _asset; break;
    case asset_sound: if (sound_exists(_asset)) return _asset; break;
    case asset_room: if (room_exists(_asset)) return _asset; break;
    case asset_background: if (background_exists(_asset)) return _asset; break;
    case asset_path: if (path_exists(_asset)) return _asset; break;
    case asset_script: if (script_exists(_asset)) return _asset; break;
    case asset_font: if (font_exists(_asset)) return _asset; break;
    case asset_timeline: if (timeline_exists(_asset)) return _asset; break;
}
return -1;

#define real_to_binary_string
/// real_to_binary_string(val)
#args val
var _str = "";
for (var i = 0; i < 23; i++)
    _str += string(int_get_bit(val, i));
return _str;

#define debug_once
///debug_once(script, [var])
if (!variable_instance_exists(id, "__debug_once_" + string(id))) {
    if (argument_count == 1) {
        return script_execute(argument[0]);
    } else {
        if (is_array(argument[1])) {
            script_execute_arr(argument[0], argument[1]);
        } else {
            script_execute_arr(argument[0], array(argument[1]));
        }
    }
    variable_instance_set(id, "__debug_once_" + string(id), true);
}

#define variable_instance_get_names_builtins
#args inst, settables_only
// Note that the alarm array cannot be gotten using variable_instance_get
var _varnames = variable_instance_get_names(inst);
if (settables_only) {
    _varnames = array_concat(_varnames, array("solid", "visible", "persistent", "depth"));
    _varnames = array_concat(_varnames, array("sprite_index", "image_alpha", "image_angle", "image_blend", "image_index", "image_number", "image_speed", "image_xscale", "image_yscale"));
} else {
    _varnames = array_concat(_varnames, array("id", "solid", "visible", "persistent", "depth", "object_index"));
    _varnames = array_concat(_varnames, array(
        "sprite_index", "sprite_width", "sprite_height", "sprite_xoffset", "sprite_yoffset",
        "image_alpha", "image_angle", "image_blend", "image_index", "image_number", "image_speed", "image_xscale", "image_yscale"));
}
return _varnames;
```