```C
#define scrUtilityArray
/// scrUtilityArray
return undefined;

#define array_cast
/// array_cast(val)
if (is_array(argument0))
    return argument0;
else
    return array(argument0);

#define array_cast_nested_2d
if (!is_array(argument0)) {
    return array(array(argument0));
} else if (is_array(argument0)) {
    if (array_length_1d(argument0) == 0) {
        return array(array_create(0));
    } else {
        for (var i = array_length_1d(argument0) - 1; i >= 0; i--) {
            if (!is_array(argument0[i])) {
                argument0[i] = array(argument0[i]);
            }
        }
        return argument0;
    }
}

#define array_cast_length
//array_cast_length(arr, len)
if (is_array(argument0)) {
    var _len = array_length_1d(argument0)
    for (var i = argument1 - 1; i >= _len; i--)
        argument0[i] = 0;
} else {
    var _val = argument0;
    for (var i = argument1 - 1; i >= 0; i--)
        argument0[i] = _val;
}
return argument0;

#define array_find
/// array_find(arr, val)
#args arr, val
for (var i = 0, n = array_length_1d(arr); i < n; i++)
    if (arr[i] == val)
        return i;
return -1;

#define array
/// array(value,...)
var r, i = argument_count;
while (--i >= 0) r[i] = argument[i];
return r;

#define array_randomize
/// array_randomize(arr);
// Randomizes all entries in an array
var
arr = argument[0],
len = array_length_1d(arr),
ind,
temp;

for (var i = 0; i < len - 1; i++)
{
    temp = arr[@ i];
    ind = i + irandom(len - i - 2) + 1;
    arr[@ i] = arr[@ ind];
    arr[@ ind] = temp;
}

#define array_create_2d
/// array_create_2d(len, h, [default])
var
length = argument[0],
height = argument[1],
_default = 0;

if (argument_count == 3)
    _default = argument[2];

var _array = -1;

for (var xx = length - 1; xx >= 0; xx--)
    for (var yy = height - 1; yy >= 0; yy--)
        _array[xx, yy] = _default;

return _array;

#define array_copy_to_grid_column
/// array_copy_to_grid_column(grid, column, arr)
#args grid, column, arr
for (var i = 0, n = array_length_1d(arr); i < n; i++)
    grid[#column, i] = arr[i];

#define array_copy_1d_to_2d
/// array_copy_1d_to_2d(arr, arr_2d_out, index)
#args arr, arr_2d_out, index
for (var i = 0; i < array_length_1d(arr); i++)
    arr_2d_out[@ index, i] = arr[i];

#define array_2d_sub
///array_2d_sub(arr_2d, index)->array
#args arr_2d, index
var
_len = array_length_2d(arr_2d, index),
_arr = array_create(_len);
for (var i = 0; i < _len; i++) {
    _arr[i] = arr_2d[index, i];
}
return _arr;

#define array_create_ext
/// array_create_ext(length, value)->array
#args length, value
var _arr = array_create(length);
for (var i = 0; i < length; i++)
    _arr[i] = value;
return _arr;

#define array_resize
///array_resize(arr, size, def)->array
#args arr, size, def
var
_len = array_length_1d(arr),
_arr;

if (_len < size) {
    _arr = arr;
    _arr[size - 1] = 0;
    for (var i = _len; i < size; i++) {
        _arr[i] = def;
    }
} else if (_len > size) {
    _arr = array_create(size);
    array_copy(_arr, 0, arr, 0, size);
} else {
    _arr = arr;
}

return _arr;

#define array_concat
/// array_concat(a, b)->array
#args a, b
var c = array_create(array_length_1d(a) + array_length_1d(b));
array_copy(c, 0, a, 0, array_length_1d(a));
array_copy(c, array_length_1d(a), b, 0, array_length_1d(b));
return c;
```
