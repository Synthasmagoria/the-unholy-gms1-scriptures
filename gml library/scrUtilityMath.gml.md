```C
#define scrUtilityMath
///scrUtilityMath(DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL)
// this is an empty script for the sake of the nested scripts
return undefined;

#define cross_product_2d
/// cross_product_2d(x1, y1, x2, y2)
return argument0 * argument3 - argument1 * argument2;

#define invlerp
/// invlerp(a, b, val)
return (argument2 - argument0) / (argument1 - argument0);
```