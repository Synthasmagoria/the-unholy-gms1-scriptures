```C
#define scrUtilityInstance
///scrUtilityInstance(DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL)
// this is an empty script for the sake of the nested scripts
return undefined;

#define instance_set_width
///instance_set_width(inst, w)
///@desc Sets the width of an instance given it has a sprite
argument[0].image_xscale = argument[1] / sprite_get_width(argument[0].sprite_index);

#define instance_set_height
///instance_set_height(inst, h)
///@desc Sets the height of an instance given it has a sprite
argument[0].image_yscale = argument[1] / sprite_get_height(argument[0].sprite_index);

#define instance_set_size
///instance_set_size(id, width, height)
argument0.image_xscale = argument1 / sprite_get_width(argument0.sprite_index);
argument0.image_yscale = argument2 / sprite_get_height(argument0.sprite_index);

#define instance_get_middle_x
///instance_get_middle_x(inst)
return
    argument0.x -
    sprite_get_xoffset(argument0.sprite_index) * argument0.image_xscale +
    argument0.sprite_width / 2;

#define instance_get_middle_y
///instance_get_middle_y(inst)
return
    argument0.y -
    sprite_get_yoffset(argument0.sprite_index) * argument0.image_yscale +
    argument0.sprite_height / 2;

#define instance_center_to_bbox_left
/// instance_center_to_bbox_left(inst)->real
if (argument0.mask_index != -1)
    return (sprite_get_bbox_left(argument0.mask_index) - sprite_get_xoffset(argument0.mask_index)) * argument0.image_xscale;
else
    return (sprite_get_bbox_left(argument0.sprite_index) - sprite_get_xoffset(argument0.sprite_index)) * argument0.image_xscale;

#define instance_center_to_bbox_right
/// instance_center_to_bbox_right(inst)->real
if (argument0.mask_index != -1)
    return (sprite_get_bbox_right(argument0.mask_index) - sprite_get_xoffset(argument0.mask_index)) * argument0.image_xscale;
else
    return (sprite_get_bbox_right(argument0.sprite_index) - sprite_get_xoffset(argument0.sprite_index)) * argument0.image_xscale;

#define instance_center_to_bbox_top
/// instance_center_to_bbox_top(inst)->real
if (argument0.mask_index != -1) {
    return (sprite_get_bbox_top(argument0.mask_index) - sprite_get_yoffset(argument0.mask_index)) * argument0.image_yscale;
} else {
    return (sprite_get_bbox_top(argument0.sprite_index) - sprite_get_yoffset(argument0.sprite_index)) * argument0.image_yscale;
}

#define instance_center_to_bbox_bottom
/// instance_center_to_bbox_bottom(inst)->real
if (argument0.mask_index != -1) {
    return (sprite_get_bbox_bottom(argument0.mask_index) - sprite_get_yoffset(argument0.mask_index)) * argument0.image_yscale;
} else {
    return (sprite_get_bbox_bottom(argument0.sprite_index) - sprite_get_yoffset(argument0.sprite_index)) * argument0.image_yscale;
}

#define instance_copy_vars
#args src, dest
var _varnames = variable_instance_get_names_builtins(src, true);
// _varnames = array_concat(_varnames, array("solid", "visible", "persistent", "depth"));
// _varnames = array_concat(_varnames, array("sprite_index", "image_alpha", "image_angle", "image_blend", "image_index", "image_number", "image_speed", "image_xscale", "image_yscale"));
for (var i = array_length_1d(_varnames) - 1; i >= 0; i--) {
    variable_instance_set(dest, _varnames[i], variable_instance_get(src, _varnames[i]));
}

#define instance_get_root_parent
#args inst
var
_object = inst.object_index,
_parent = object_get_parent(_object);

if (_parent == -1) {
    return inst.object_index;
} else {
    do {
        _object = _parent;
        _parent = object_get_parent(_object);
    } until (_parent == -1);
    return _object;
}

#define instance_get_all_ext
///instance_get_all(obj_array)->array
#args obj_arr
var _instance_number = 0;
for (var i = array_length_1d(obj_arr) - 1; i >= 0; i--) {
    _instance_number += instance_number(obj_arr[i]);
}
var _instances = array_create(_instance_number);
var _count = 0;
for (var i = array_length_1d(obj_arr) - 1; i >= 0; i--) {
    with (obj_arr[i]) {
        _instances[_count] = id;
        _count++;
    }
}
return _instances;

#define instance_set_tag
///instance_set_tag(inst_id, tag)
variable_instance_set(argument0, "__internal_utility_tag_" + argument1, true);

#define instance_find_by_tag
#args obj, tag
var _tag = "__internal_utility_tag_" + tag;
with (obj) {
    if (variable_instance_exists(id, _tag)) {
        return id;
    }
}
return noone;

#define instance_find_by_tag_all
#args obj, tag, out
var
_tag = "__internal_utility_tag_" + tag,
_number = 0;
with (obj) {
    if (variable_instance_exists(id, _tag)) {
        ds_list_add(out, id);
        _number++;
    }
}
return _number;

#define instance_neighboring_assign_groups
///instance_neighboring_assign_groups(obj, groupvarname)
global.__instance_neighboring_groupid = 0;
global.__instance_neighboring_groupvarname = argument1;

if (!variable_global_exists("__instance_neighboring")) {
    global.__instance_neighboring = ds_list_create();
} else {
    ds_list_clear(global.__instance_neighboring);
}

with (argument0) {
    __instance_neighboring_recurse = true;
}

with (argument0) {
    if (__instance_neighboring_recurse) {
        ds_list_add_list(global.__instance_neighboring, ds_list_create());
        _instance_neighboring_recursive_group_assign();
        global.__instance_neighboring_groupid++;
    }
}

return global.__instance_neighboring_groupid;

#define _instance_neighboring_recursive_group_assign
///_instance_neighboring_recursive_group_assign()
__instance_neighboring_recurse = false;

with (object_index)
    if (__instance_neighboring_recurse) && instance_place(x-1,y,other.id)
        _instance_neighboring_recursive_group_assign();
with (object_index)
    if (__instance_neighboring_recurse) && instance_place(x+1,y,other.id)
        _instance_neighboring_recursive_group_assign();
with (object_index)
    if (__instance_neighboring_recurse) && instance_place(x,y-1,other.id)
        _instance_neighboring_recursive_group_assign();
with (object_index)
    if (__instance_neighboring_recurse) && instance_place(x,y+1,other.id)
        _instance_neighboring_recursive_group_assign();

variable_instance_set(id, global.__instance_neighboring_groupvarname, global.__instance_neighboring_groupid);
ds_list_add(ds_list_find_value(global.__instance_neighboring, global.__instance_neighboring_groupid), id);
```