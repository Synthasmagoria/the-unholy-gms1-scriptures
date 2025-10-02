### Set image_blend in the room editor
It is possible to set image_blend in the room editor. It shows up visually.
![[editor image blend.png]]

### Set image_angle in the room editor
Rotating instances in the room editor will make it very slow and unresponsive. Never do it.
### Set size of, and preview texture pages
Global Game Settings -> Windows -> Graphics
![[texture page size.png]]
The preview button will build all texture pages and open them in a folder view for you to look at.
This can be useful for debugging shaders that manipulate textures in weird ways.

### Texture trimming
Be aware that if your sprites have transparent borders, GMS will trim them.
If you don't keep this in mind when making e.g. outline shaders, then you wont be able to draw on those transparent parts of the image, because they've been left out. In order to prevent the trimming it can either be disabled for specific texture groups in `Resources -> Global Game Settings -> Texture Groups`.
![[disable texture cropping in group.png]]
Or alternatively you can place a nearly fully transparent pixel in the top left and bottom right corners of the sprite.
![[single sprite image cropping workaround.png]]

### Room script global builtins
It is possible to access builtins such as object_index and id in room start script. However they will be set to 0.