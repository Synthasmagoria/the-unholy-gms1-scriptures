### "Fog trick"
The following script makes it possible to draw anything as a colored silhouette.
```C
/// fog_trick(col, amt)
if (argument_count == 2) {
	var amount = argument[1];
	d3d_set_fog(1, argument[0], (1 - amount) * 16000, (2 - amount) * 16000);
} else {
	d3d_set_fog(0,0,0,0);
}
```

### HLSL9
It is possible to write HLSL9 shaders instead of GLSL.
This has the benefit of removing the transpilation step that occurs when targeting Windows.
![[hlsl9.png]]
The following is a GMS1.4.9999 HLSL9 passthrough shader.
VERTEX:
```hlsl
struct VS_INPUT {
    float4 vPos   : POSITION;
    //float4 vNormal : NORMAL;
    float4 vColor : COLOR;
    float2 vTexCoord : TEXCOORD0;
};

struct PS_INPUT {
    float4 Position : POSITION;  // interpolated vertex position (system value)
    float4 Color    : COLOR;       // interpolated diffuse color
    float2 TexCoord : TEXCOORD0;
};

PS_INPUT main(VS_INPUT input) // main is the default function name {
    PS_INPUT Output;
	
    // Transform the position from object space to homogeneous projection space
    Output.Position = mul(gm_Matrices[MATRIX_WORLD_VIEW_PROJECTION], input.vPos);

    // Just pass through the color data
    Output.Color = input.vColor;
    Output.TexCoord = input.vTexCoord;
 
    return Output;
}
```
FRAGMENT:
```hlsl
struct PS_INPUT {
    float4 Color    : COLOR;
    float2 TexCoord : TEXCOORD0;
};

struct PS_OUTPUT {
    float4 Color : COLOR;
};

PS_OUTPUT main(PS_INPUT In) {
	PS_OUTPUT Output;
	Output.Color = In.Color * tex2D(gm_BaseTexture, In.TexCoord);
	return Output;
}
```

### Unexposed API
The collision list functions exist in Gamemaker Studio 1, but have not been exposed as part of the API. But you can enable them by creating an empty extension exposing the function names.

![[collision_point_list definition.png]]

The drawback is that it will fail when compiling for YYC.
See resources for pre-made extensions.

### Double buffer
```C
if (!surface_exists(surf_a)) {
    surf_a = surface_create(800, 608);
    surface_set_target(surf_a);
    draw_clear_alpha(c_black, 0.0);
    draw_sprite_ext(sprP2Background, 0, 0, 0, 2, 2, 0, c_white, 1.0);
    surface_reset_target();
}

if (!surface_exists(surf_b)) {
    surf_b = surface_create(800, 608);
    surface_set_target(surf_b);
    draw_clear_alpha(c_black, 0.0);
    surface_reset_target();
}

surface_set_target(surf_b);
shader_set(sh);
draw_surface(surf_a, 0, 0);
shader_reset();
surface_reset_target();

var _temp = surf_a;
surf_a = surf_b;
surf_b = _temp;

draw_surface(surf_a, 0, 0);

surface_set_target(surf_b);
draw_clear_alpha(c_black, 0.0);
surface_reset_target();
```
### DLLs
DLL template. Should be possible to adapt to GMS1.4.9999:
https://github.com/YAL-GameMaker/GMSDLL
Pass GMS function addresses to DLL with this:
https://github.com/YAL-GameMaker/function_get_address

### YYC
*still need to do research on this*
https://forum.gamemaker.io/index.php?threads/gamemaker-1-4-yyc-and-visual-studio-related-stuff.77067/

### 'var' is not function-scoped
In instances 'var' is event scoped.
In scripts 'var' is script scoped.
That makes it possible to write code like this:
```C
for (var i = 0; i < 2; i++) {
	if (i == 1) {
		show_debug_message(variable);
	}
	var variable = "Hello";
}
show_debug_message(variable + "wowie");
```
This code will print Hello, then Hellowowie to the console.

### 'begin' and 'end' serve as '{' and '}'
This legacy syntax still works in GMS1.4.9999
```C
if (false) begin
    show_debug_message("this works!");
end else if (true) begin
    show_debug_message("compiles just fine!");
end 
```
You can even mix the syntaxes
```C
if (false) {
    show_debug_message("this works!");
end else if (true) begin
    show_debug_message("compiles just fine!");
}
```

### Bitwise operators treat literals differently from variables
When you do bitwise operations with an lvalue real on the left and any type of real value on the right it will return an int64.
```C
var a = 1;
var b = a << 1;
if (is_int64(b)) {
	show_debug_message("b is int64");
}
```
However, when the left value of the bitwise operation is an rvalue it will return a real.
```C
var a = 1 << 1;
if (is_real(a)) {
	show_debug_message("a is real");
}
```
This behavior is not consistent however. At values starting from 31 on the left shift operator it will start returning an int64 instead.
```C
var a;
a = 1 << 30;
if (is_real(a)) {
	show_debug_message("a is real");
}
a = 1 << 31;
if (is_int64(a)) {
	show_debug_message("a is int64");
}
```
This might be because 32-bit has some significance in doubles (real's underlying value). But I'm not familiar enough with [Double-precision floating-point format](https://en.wikipedia.org/wiki/Double-precision_floating-point_format)To be able to say for sure.

### It is possible to trigger overflow in an int64 without getting an error
`value = (~int64(0) ^ (int64(1) << 63)) + 1;`

### Nested 2d arrays and 2d arrays are not the same
In Gamemaker Studio 1 all arrays are 2D arrays by default.
You can see this by creating an array and calling array_height_2d or show_debug_message on it.
```C
var _arr = array_create(1);
show_debug_message(_arr);
show_debug_message(array_height_2d(_arr));
show_debug_message(array_length_2d(_arr, 0));
show_debug_message(_arr[0]);
show_debug_message(_arr[0, 0]);
```
The above code prints this to the console
```
{ { 0 },  }
1
1
0
0
```

This is the reason for why creating an array inside an array is different from using an array as a 2D array.
```C
// This is a regular 2d array
var _a = array_create(1);
// It can be gotten as such
show_debug_message(_a[0, 0]);

// This is a nested array
var _b = array_create(1);
_b[0] = array_create(1);
// It can be gotten as such
var _nested = _b[0];
show_debug_message(_nested[0]);
```

While the first dimension of an array can be filled automatically by setting the last value in the array, the second dimension will contain empty arrays unless explicitly initialized.
```C
// Improper 2d array initialization
var _a;
_a[6] = 3;
show_debug_message(_a);
_a[3, 3] = 3;
show_debug_message(_a);
```

```
{ { 0,0,0,0,0,0,3 },  }
{ { 0,0,0,0,0,0,3 }, {  }, {  }, { 0,0,0,3 },  }
```

Regular arrays don't let you split off parts of the 2d array into separate arrays. Accessing the array using just the first part of the indexer will result in the values of the first array in the 2d array to be gotten.
```C
var _a = array_create(0);
_a[0, 0] = 100;
_a[0, 1] = 200;
_a[3, 3] = 3;
show_debug_message(_a);
show_debug_message(_a[0]);
show_debug_message(_a[1]);
```

```
{ { 100,200 }, {  }, {  }, { 0,0,0,3 },  }
100
200
```

Instead you'd have to create a new loop and manually shave off the part of the array that you want as a separate array.

Nested arrays are memory safe, meaning they will be freed from memory when an instance that has one gets destroyed. Nested arrays also get printed to the console properly when passed to show_debug_message. But nested arrays' deep members are not accessible using one line of code.
```C
var a = array(array(10, 0), array(5, 2));
```
`a[1, 0]` - Will try to access the first member of the second dimension of the base array which does not exist and will therefore throw an error.
`a[1][0]` - Is not valid syntax in GMS1.4.9999 an will therefore cause a compile time error.
Instead you'll have to do this:
```C
var b = a[1];
show_debug_message(b[0]);
```

```
5
```

### Marking invalid list index as list close without error
This snipped will cause your game to close without an error
```C
list = ds_list_create();
ds_list_mark_as_list(list, -1);
```