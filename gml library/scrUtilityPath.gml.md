```C
#define scrUtilityPath
///scrUtilityPath(DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL)
// this is an empty script for the sake of the nested scripts
return undefined;

#define path_triangulate
/// path_triangulate(path)->ds_list
#args path
var _vertex_number = path_get_number(path);
var _triangle_list = ds_list_create();

if (_vertex_number < 3) {
    return _triangle_list;
}

var _index_list = ds_list_create();
for (var i = 0; i < _vertex_number; i++) {
    ds_list_add(_index_list, i);
}

var
a, b, c,
vax, vay, vbx, vby, vcx, vcy,
vax_to_vbx, vay_to_vby, vax_to_vcx, vay_to_vcy;
while (ds_list_size(_index_list) > 3) {
    for (var i = 0; i < ds_list_size(_index_list); i++) {
        a = ds_list_find_value(_index_list, (i - 1) + ds_list_size(_index_list) * (i == 0));
        b = ds_list_find_value(_index_list, i);
        c = ds_list_find_value(_index_list, (i + 1) % ds_list_size(_index_list));
        
        vax = path_get_point_x(path, a);
        vay = path_get_point_y(path, a);
        vbx = path_get_point_x(path, b);
        vby = path_get_point_y(path, b);
        vcx = path_get_point_x(path, c);
        vcy = path_get_point_y(path, c);
        
        vax_to_vbx = vbx - vax;
        vay_to_vby = vby - vay;
        vax_to_vcx = vcx - vax;
        vay_to_vcy = vcy - vay;
        
        if (cross_product(vax_to_vbx, vay_to_vby, vax_to_vcx, vay_to_vcy) < 0) {
            continue;
        }
        
        var _point_in_triangle = false, _pi;
        for (var j = 0; j < ds_list_size(_index_list); j++) {
            _pi = ds_list_find_value(_index_list, j);
            if (_pi == a or _pi == b or _pi == c) {
                continue;
            }
            
            if (point_in_triangle(
                path_get_point_x(path, _pi), path_get_point_y(path, _pi),
                vax, vay, vbx, vby, vcx, vcy)) {
                _point_in_triangle = true;
                break;
            }
        }
        
        if (_point_in_triangle) {
            continue;
        }
        
        ds_list_add(_triangle_list, a, b, c);
        ds_list_delete(_index_list, i);
        i--;
    }
}

ds_list_destroy(_index_list);

return _triangle_list;

```