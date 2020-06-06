<h1 align="center">Xev File Format Specification</h1>

Xev is a XML-based format similar to ora and kra.<br/>
It is meant to be a more open replacement for enve binary ev format.

## File Layout Specification

### Storage

Xev files have the file name extension .xev. The data is stored within a Zip file wrapper.

### Files

```
example.xev
├ mimetype
├ document.xml
├ UI/
│  └ layouts.xml
├ Thumbnails/
│  └ thumbnail.png
└ scenes/
   ├ 0/
   │  ├ stack.xml
   │  ├ properties.xml
   │  ├ gradients.xml
   │  └ objects/
   │    ├ 0/
   │    │  ├ stack.xml
   │    │  ├ properties.xml
   │    │  ├ assets/
   │    │    └ [PNG images (if any) referenced by properties.xml]
   │    │  └ objects/
   │    │    └ [child objects (if any)]
   │    ├ 1/
   │    │  └ (...)
   │    └ (...)
   ├ 1/
   │  └ (...)
   └ (...)

```

#### mimetype

The first file in the archive must be called "mimetype", without a file name extension. It must be STORED without compression. This file must contain the string "application/enve", with no whitespace or trailing newline.

#### Thumbnails/thumbnail.png

A xev file must have a thumbnail.png in order to allow file browser software to render the thumbnail efficiently. It must be a non-interlaced PNG with 8 bits per channel of at most 256x256 pixels. It should be as big as possible without upscaling or changing the aspect ratio. Any aspect ratio is permitted. It should not contain any frame or decoration.

<h4><a name="document.xml">document.xml</a></h4>

```xml
<Document format-version="0">
    
    <!-- bookmarked colors -->
    <ColorBookmarks>
        <Color name="#000000"/>
        <!-- more colors -->
    </ColorBookmarks>
    
    <!-- bookmarked libmypaint brushes -->
    <BrushBookmarks>
        <Brush collection="Classic" name="blending_knife"/>
        <!-- more brushes -->
    </BrushBookmarks>
    
    <!-- scenes declaration -->
    <Scenes>
        <!-- scene with stack index 0 -->
        <Scene name="Example" width="1920" height="1080" fps="24"
               frame="101" clip="true" resolution="1"/>
        <!-- more scenes, with consecutive stack indexes -->
    </Scenes>
</Document>
```

#### scenes/X/

**X** - stack index, based on the order of appearence in <a href="#document.xml">document.xml</a>.
<br/>
All properties and objects contained in the scene are specified in this folder.
<br/>
The content of this folder is similar to a <a href="#Group Object">Group Object's</a> <a href="#scenes/X/objects/Y/">scenes/X/objects/Y/</a> folder.
The only difference is that a scene folder contains **gradients.xml**.

##### scenes/X/gradients.xml


This file declares all gradients used by scene's child objects.

```xml
<Gradients>
    <Gradient id="0">
        <Color mode="0">
            <Red value="1"/>
            <Green value="0"/>
            <Blue value="0"/>
            <Alpha value="1"/>
        </Color>
        <Color mode="0">
            <Red value="1"/>
            <Green value="1"/>
            <Blue value="1"/>
            <Alpha value="1"/>
        </Color>
        <!-- more colors -->
    </Gradient>
    <!-- more gradients -->
</Gradients>
```
##### scenes/X/properties.xml

See <a href="#properties.xml">properties.xml</a>.
Defines scene background color.

```xml
<Object id="0">
   <!-- background color -->
   <Color mode="0">
      <Red value="1"/>
      <Green value="1"/>
      <Blue value="1"/>
      <Alpha value="1"/>
   </Color>
</Object>
```

<h4><a name="scenes/X/objects/Y/">scenes/X/objects/Y/</a></h4>

**Y** - object stack index, based on the order of appearence in <a href="#stack.xml">stack.xml</a>.
<br/>

<h4><a name="properties.xml">scenes/X/objects/Y/properties.xml</a></h4>

Defines <a href="#Object">object's</a> properties.

<h4><a name="stack.xml">stack.xml</a></h4>

Declares children order, names and types.

```xml
<Stack>
    <Object name="Object 1" type="5"/>  <!-- stack index: 0 -->
    <Object name="Path 3" type="0"/>  <!-- stack index: 1 -->
    <!-- more objects -->
</Stack>
```

##### objects/

Contains child object folders, named according to object stack indexes, which correspond to their order in **stack.xml**.

## Syntax

<h3><a name="Object">Object</a></h3>

<a href="#properties.xml">properties.xml</a>
```xml
<Object id="0">
   <Transform>
      <Translation>
         <X value="0"/>
         <Y value="0"/>
      </Translation>
      <Rotation value="0"/>
      <Scale>
         <X value="1"/>
         <Y value="1"/>
      </Scale>
      <Shear>
         <X value="0"/>
         <Y value="0"/>
      </Shear>
      <Pivot>
         <X value="0"/>
         <Y value="0"/>
      </Pivot>
      <Opacity value="100"/>
   </Transform>
   
   <Properties>
      <!-- properties -->
   </Properties>
   
   <BlendEffects>
      <!-- blend effects -->
   </BlendEffects>
   
   <RasterEffects>
      <!-- raster effects -->
   </RasterEffects>
   
   <!-- path effects are supported only by: -->
   <!-- groups, layers, paths, ellipses, rectangles, text -->
   <PathBaseEffects>
      <!-- path effects -->
   </PathBaseEffects>
   <FillEffects>
      <!-- path effects -->
   </FillEffects>
   <OutlineBaseEffects>
      <!-- path effects -->
   </OutlineBaseEffects>
   <OutlineEffects>
      <!-- path effects -->
   </OutlineEffects>
   
   <!-- object type specific properties -->
</Object>
```

Each object has its own xev-file-wide unique **id**.
<br/>
Ids of the timeline widgets in which property content is shown is saved in **open** attribute.
<br/>

There are many types of objects. The type is defined in parent's <a href="#stack.xml">stack.xml</a>

**type**
* **0** - <a href="#Path Object">Path Object</a>
* **1** - <a href="#Ellipse Object">Ellipse Object</a>
* **2** - <a href="#srcObjects">Image Object</a>
* **3** - <a href="#Rectangle Object">Rectangle Object</a>
* **4** - <a href="#Text Object">Text Object</a>
* **5** - <a href="#Group Object">Layer Object</a>
* **6** - Scene Object (will not appear in stack.xml)
* **7** - <a href="#linkObjects">Object link Object</a>
* **8** - <a href="#linkObjects">Group link Object</a>
* **9** - <a href="#linkObjects">Scene link Object</a>
* **10** - <a href="#srcObjects">SVG link Object</a>
* **11** - <a href="#videoObjects">Video Object</a>
* **12** - <a href="#imgSeqObjects">Image Sequence Object</a>
* **13** - <a href="#Paint Object">Paint Object</a>
* **14** - <a href="#Group Object">Group Object</a>
* **15** - Custom Object

<h4><a name="Path Object">Path Object</a></h4>

<h4><a name="Ellipse Object">Ellipse Object</a></h4>

<h4><a name="Rectangle Object">Rectangle Object</a></h4>

<h4><a name="Text Object">Text Object</a></h4>

<a href="#properties.xml">properties.xml</a>
```xml
<Object fontSlant="0" fontWeight="400" hAlign="1" fontSize="72" fontWidth="5" fontFamily="Arial" id="1" vAlign="32">
   <!-- object properties -->
   <Fill type="1">
      <Color mode="0">
         <Red value="0"/>
         <Green value="0"/>
         <Blue value="0"/>
         <Alpha value="1"/>
      </Color>
   </Fill>
   <Outline type="0" stroke-linejoin="round" stroke-linecap="round">
      <Width value="1"/>
   </Outline>
   
   <Text value="example text"/>
   <Spacing>
      <Letters value="0"/>
      <Words value="1"/>
      <Lines value="1"/>
   </Spacing>
   <TextEffects/>
</Object>
```

<h4><a name="Group Object">Group/Layer Object</a></h4>

<a href="#properties.xml">properties.xml</a>
```xml
<Object id="1">
   <!-- object properties -->
   <FlipBook checked="false">
      <Index value="0"/>
   </FlipBook>
</Object>
```

<h4><a name="srcObjects">SVG Link/Image Object</a></h4>

<a href="#properties.xml">properties.xml</a>
```xml
<Object id="1" srcAbs="/home/username/Documents/folder/(...)" srcRel="folder/(...)">
   <!-- object properties -->
</Object>
```

<h4><a name="imgSeqObjects">Image Sequence</a></h4>

<a href="#properties.xml">properties.xml</a>
```xml
<Object id="1" srcAbs="/home/username/Documents/folder/(...)" srcRel="folder/(...)">
   <!-- object properties -->
   <Frame enabled="false" value="0"/> <!-- frame remapping -->
</Object>
```

<h4><a name="videoObjects">Video Object</a></h4>

<a href="#properties.xml">properties.xml</a>
```xml
<Object id="1" absSrc="/home/username/Documents/folder/(...)" relSrc="folder/(...)">
   <!-- object properties -->
   <Frame enabled="false" value="0"/> <!-- frame remapping -->
   <Sound>
      <Volume value="100"/>
   </Sound>
</Object>
```

<h4><a name="linkObjects">Object/Group/Scene Link Object</a></h4>

<a href="#properties.xml">properties.xml</a>
```xml
<Object id="2">
   <!-- object properties -->
   <LinkTarget targetId="3"/>
</Object>
```
**ObjectLink** **targetId** corresponds to target **Object**'s **id**.

<h4><a name="Paint Object">Paint Object</a></h4>

<a href="#properties.xml">properties.xml</a>
```xml
<Object id="1">
   <!-- object properties -->
   <Canvas pivot="64 64"/> <!-- assets/Canvas -->
</Object>
```
**pivot** - specifies position of (0, 0) pixel. If Canvas is animated there are multiple values seperated by comas, e.g., `64 64, 128 128`

<h5>Image Assets</h5>

Paint Object images are saved in Canvas property assets folder `(...)/assets/Canvas/`, e.g., `/scenes/0/objects/0/assets/Canvas/`.

There is only a single image for not animated PaintSurface - `value.png`.
For animated PaintSurface, image names correspond to the frames they are on, e.g., `0.png` for a keyframe on frame 0.


<h3><a name="Properties">Properties</a></h3>

<h4><a name="Transform">Transform</a></h4>

<h4><a name="Color">Color</a></h4>

<h4><a name="Gradient">Gradient</a></h4>

<h4><a name="Path">Path</a></h4>

<h4><a name="Static Group Property">Static Group Property</a></h4>

<h4><a name="Dynamic Group Property">Dynamic Group Property</a></h4>

<h4><a name="Paint Canvas">Paint Canvas</a></h4>

<h4><a name="Text">Text</a></h4>


