```C
#define scrUtilityDsList
///scrUtilityDsList
return undefined;

#define ds_list_add_map
/// ds_list_add_map(index, map)
#args index, map
ds_list_add(index, map);
ds_list_mark_as_map(index, ds_list_size(index) - 1);

#define ds_list_add_list
/// ds_list_add_list(index, list)
#args index, list
ds_list_add(index, list);
ds_list_mark_as_list(index, ds_list_size(index) - 1);

#define ds_list_resize
///ds_list_resize(list, new_size, [default])
#args list, new_size,
var
_def = 0,
_current_size = ds_list_size(list);
if (argument_count > 2) {
    _def = argument[2];
}
if (new_size < 0) {
    show_error("Cannot resize ds_list to be smaller than 0", true);
}
if (new_size < _current_size) {
    repeat (_current_size - new_size) {
        ds_list_delete(list, _current_size - 1);
        _current_size--;
    }
} else if (new_size > _current_size) {
    for (var i = new_size - 1; i >= _current_size; i--) {
        list[|i] = _def;
    }
}

#define ds_list_append_list
/// ds_list_append_list(list, source)
#args list, source
for (var i = 0, n = ds_list_size(source); i < n; i++) {
    ds_list_add(list, ds_list_find_value(source, i));
}

#define ds_list_append_array
/// ds_list_append_array(list, arr)
#args list, arr
for (var i = 0, n = array_length_1d(arr); i < n; i++) {
    ds_list_add(list, arr[i]);
}

#define ds_list_array
/// ds_list_array(list)->array
#args list
var _arr;
for (var i = ds_list_size(list) - 1; i >= 0; i--) {
    _arr[i] = list[|i];
}
return _arr;
```