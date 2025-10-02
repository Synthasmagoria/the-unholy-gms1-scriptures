```C
#define scrUtilityBitwise
///scrUtilityBitwise(DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL)
// this is an empty script for the sake of the nested scripts
return undefined;

#define int_get_bit
///int_get_bit(int, bit_index)->int64
return (argument0 >> argument1) & 1;

#define int_truth_bit
///int_truth_bit(int, bit_index)->int64
return argument0 | (1 << argument1);

#define int_create_mask
///int_create_mask(offset, length)->int64
return ~(~0 << argument[1]) << argument[0];

#define int_set_bit
///int_set_bit(int, bit_index, bool)->int64
return argument0 & ~(1 << argument1) | argument2 << argument1;

#define int_set_bits
///int_set_bits(int, offset, length, value)->int64
return argument0 & ~int_create_mask(argument1, argument2) | argument3 << argument1;

#define int_get_bits
///int_get_bits(int, offset, length)->int64
return argument0 >> argument1 & int_create_mask(0, argument2);
```