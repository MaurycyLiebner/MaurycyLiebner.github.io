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
        <!-- scene with id 0 -->
        <Scene name="Example" width="1920" height="1080" fps="24"
               frame="101" clip="true" resolution="1"/>
        <!-- more scenes, with consecutive ids -->
    </Scenes>
</Document>
```

#### scenes/X/

**X** - scene id, based on the order of appearence in <a href="#document.xml">document.xml</a>.
<br/>
All properties and objects contained in the scene are specified in this folder.
<br/>
The content of this folder is similar to a <a href="#groupObject">Group Object</a> folder.
The only difference is that scene folder contains **gradients.xml**.

##### scenes/X/gradients.xml

This file declares all gradients used by scene's child objects.

```xml
<Gradients>
    <Gradient id="0">
        <Color mode="0">
            <V1 value="1"/>
            <V2 value="0"/>
            <V3 value="0"/>
            <A value="1"/>
        </Color>
        <Color mode="0">
            <V1 value="1"/>
            <V2 value="1"/>
            <V3 value="1"/>
            <A value="1"/>
        </Color>
        <!-- more colors -->
    </Gradient>
    <!-- more gradients -->
</Gradients>
```
#### scenes/X/objects/Y/

**Y** - object id, based on the order of appearence in <a href="#stack.xml">stack.xml</a>.
<br/>

<h4><a name="object">Object</a></h4>

##### properties.xml

```xml
<Object id="1" open="0 1">
    <CustomProperties/>
    <BlendEffects/>
    <Transform open="0">
        <Translation>
            <X value="711.5446680890581"/>
            <Y value="375.5977814084237"/>
        </Translation>
        <Scale>
            <X value="3.680336265192215"/>
            <Y value="3.680336265192215"/>
        </Scale>
        <Rotation value="0"/>
        <Pivot>
            <X value="253.9383754730225"/>
            <Y value="156.3928337097168"/>
        </Pivot>
        <Shear>
            <X value="0"/>
            <Y value="0"/>
        </Shear>
        <Opacity value="100"/>
    </Transform>
    <RasterEffects/>
    <PathEffects/> <!-- base path effects -->
    <PathEffects/> <!-- fill path effects -->
    <PathEffects/> <!-- outline base path effects -->
    <PathEffects/> <!-- outline path effects -->
</Object>
```
Each object has its own xev-file-wide unique **id**.
<br/>
Ids of the timeline widgets in which property content is shown is saved in **open** attribute.
<br/>

<h4><a name="groupObject">Group Object</a></h4>

Extends <a href="#object">Objects</a>.
<br/>
The difference between group objects and regular objects is that group objects contain other objects.
<br/>
The group object's children stack is defined by **stack.xml**, and all child object data resides in **objects/** folder.

##### stack.xml

Declares children order, names and types.

```xml
<Stack>
    <Object name="Object 1" type="5"/>  <!-- id: 0 -->
    <Object name="Path 3" type="0"/>  <!-- id: 1 -->
    <!-- more objects -->
</Stack>
```

**type** - object type
* 0 - path object
* 1 - ellipse
* 2 - image object
* 3 - rectangle
* 4 - text object
* 5 - layer object
* 6 - scene (will not appear in stack.xml)
* 7 - object link
* 8 - group object link
* 9 - scene link
* 10 - SVG link object
* 11 - video object
* 12 - image sequence object
* 13 - paint object
* 14 - group object
* 15 - custom object
* 16 - sculpt path object

##### objects/

Contains child object folders, named according to object ids, which correspond to their order in **stack.xml**.
