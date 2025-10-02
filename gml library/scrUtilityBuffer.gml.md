```C
#define scrUtilityBuffer
///scrUtilityBuffer(DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL, DO_NOT_CALL)
// this is an empty script for the sake of the nested scripts
return undefined;

#define buffer_read_string
/// buffer_read_string(buffer, bytes)->string
#args buffer, bytes
var _prev_pos = buffer_tell(buffer);
var _str = buffer_read(buffer, buffer_text);
buffer_seek(buffer, buffer_seek_relative, _prev_pos - buffer_tell(buffer) + bytes);
return _str;

#define buffer_write_string_aligned
/// buffer_write_string_aligned(buffer, str, bytes)
#args buffer, str, bytes
var _prev_pos = buffer_tell(buffer);
buffer_write(buffer, buffer_text, str);
buffer_seek(buffer, buffer_seek_relative, _prev_pos - buffer_tell(buffer) + bytes);

```