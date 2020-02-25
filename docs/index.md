<h1 align="center">Shader Effects</h1>

Shader effects let users create their own raster effects. You need two files to get started with shader effects.

* The <a href="https://www.w3schools.com/xml/xml_whatis.asp">XML</a> **\*.gre** file defines the properties for the effect.
* The <a href="https://www.khronos.org/opengl/wiki/Fragment_Shader">GLSL fragment shader</a> **\*.frag** file defines the effect for a given set of property values.

To use shader effects, both **\*.gre** and **\*.frag** files have to be placed in **`home_directory/.enve/ShaderEffects`** directory.
<br/>
The **\*.gre** and **\*.frag** files have to share a name, e.g., **`exampleEffect.gre`** and **`exampleEffect.frag`**.
<br/>

<p align="center"><a href="https://github.com/MaurycyLiebner/enve/tree/master/examples/shaderEffects">Here you can find modest examples</a></p>
<br/>
<h2 align="center">Define Properties (*.gre file)</h2>

<h3>Root Element</h3>

```xml
<ShaderEffect name="Example Effect">
  <!-- properties and values -->
</ShaderEffect>
```

A **ShaderEffect** <a href="https://en.wikipedia.org/wiki/Root_element">root element</a> has only a **name** attribute. The **name** will be visible in enve interface.<br/>
The **ShaderEffect** encloses all properties and values for the effect.

<h3>Properties</h3>

There are three types of properties:
* **int** - integer properties
* **float** - floating-point number properties
* **vec2** - two-value float properties

All the properties are animatable, meaning their values can change with time.

```xml
<ShaderEffect name="Example Effect">
    <Property name="exampleProperty" nameUI="example property"
              type="float" min="0" max="100" ini="8" step="1"
              glValue="true" resolutionScaled="true"/>
</ShaderEffect>
```
![example](https://user-images.githubusercontent.com/16670651/75267284-56ea7a00-57f6-11ea-84ef-ed697f2720d6.png)


<h4>Property Attributes</h4>

* **name** - name that will be used to reference the property, cannot contain spaces or special characters, e.g., **`exampleProperty`**
* **nameUI** - name that will be visible in enve interface, e.g., **`example property`**
* **xnameUI** - name of the x component of **vec2**
* **ynameUI** - name of the y component of **vec2**
* **type** - type of the property, e.g., **`int`**, **`float`**, **`vec2`**
* **min** - minimum value that can be assigned to the property, e.g., **`0`**, **`[0, 0]`**
* **max** - maximum value that can be assigned to the property, e.g., **`100`**, **`[100, 50]`**
* **ini** - initial value for the property, e.g., **`55`**, **`[75, 25]`**
* **step** - recommended value increment for user interactions with the interface, e.g., **`1`**, **`[1, 1]`**
* **glValue** - specifies whether the property is used in the fragment shader, e.g., **`true`**, **`false`**
* **resolutionScaled** - specifies whether the property value should be multiplied by scene resolution, e.g., **`true`**, **`false`**

As you might have guessed, the **`[x, y]`** attribute values are only supported by **vec2** type, **vec2** also accepts **`x`**, that will automatically be expanded to **`[x, x]`**.

<h4>Default Attribute Values</h4>

* **name** - no default value, cannot be omitted
* **nameUI** - the same value as **name**
* **xnameUI** - **`x`**
* **ynameUI** - **`y`**
* **type** - no default value, cannot be omitted
* **min** - **`0`**
* **max** - **`100`**
* **ini** - **`0`**
* **step** - **`1`**
* **glValue** - **`false`**
* **resolutionScaled** - **`false`**

<h3>Values</h3>

```xml
<ShaderEffect name="Example Effect">
    <Property name="exampleProperty" nameUI="example property"
              type="float" min="0" max="100" ini="8" step="1"
              glValue="true" resolutionScaled="true"/>
    <Value name="exampleValue" type="float"
           value="0" glValue="true"/>
</ShaderEffect>
```
**Values** have no representation in the user interface. Instead, they let you define constants, and perform calculations on properties and other values.

```xml
<ShaderEffect name="Rotate">
    <Property name="angleDegrees" nameUI="angle"
              type="float" min="0" max="360" ini="0" step="1"/>
    <Value name="angleRadians" type="float"
           value="angleDegrees*3.1416/180" glValue="true"/>
</ShaderEffect>
```
A good example is converting angle values from degrees to radians.

```xml
<ShaderEffect name="Rotate">
    <Property name="angleDegrees" nameUI="angle"
              type="float" min="0" max="360" ini="0" step="1"/>
    <Value name="angleRadians" type="float" glValue="true">
        angleDegrees*3.1416/180
    </Value>
</ShaderEffect>
```
In the example above, only the value in degrees will be visible to the user, and only the value in radians will be passed to the fragment shader.
<br/>
You can also define scripts by enclosing them inside **`<Value></Value>`**, instead of using the **value** attribute.
<br/>
<h4>Attributes</h4>

* **name** - name of that will be used to reference the value, cannot contain spaces or special characters, e.g., **`exampleValue`**
* **type** - type of the value, e.g., **`int`**, **`float`**, **`vec2`**
* **value** - specifies the value, e.g., **`0`**, **`exampleProperty*5`**, **`[exampleVec2Property[0]*5, exampleVec2Value[1]*5]`**
* **glValue** - specifies whether the value is used in the fragment shader, e.g., **`true`**, **`false`**(default)
<br/>

```xml
<ShaderEffect name="Rotate">
    <Property name="angleDegrees" nameUI="angle"
              type="float" min="0" max="360" ini="0" step="1"/>
    <Value name="angleRadians" type="float" glValue="true">
        angleDegrees*3.1416/180
    </Value>
    <Value name="angleRadians2" type="float" value="2*angleRadians"/>
</ShaderEffect>
```

Please note that the order in which values are defined matters. Values should be defined before they are used. In the example above, the order of **`angleRadians`** and **`angleRadians2`** cannot be reversed.

<h3>Margin</h3>

Some effects might need to expand the texture size, e.g., blur effects require additional space depending on the radius.
Enve will expand the texture for you, all you have to do is define the Margin.

![exampleMargin](https://user-images.githubusercontent.com/16670651/75272974-34a92a00-57ff-11ea-8344-b9a5e8ab4868.png#center)

```xml
<ShaderEffect name="Example Blur">
    <Property name="blurRadius" nameUI="blur radius"
              type="float" min="0" max="100" ini="0" step="1"
              glValue="true" resolutionScaled="true"/>
    <Margin value="blurRadius"/>
</ShaderEffect>
```
The Margin only has the **value** attribute.<br/>
The Margin can be defined with four values **`[left, top, right, bottom]`**,
<br/>
with two values **`[horizontal, vertical]`**, which translates to **`[horizontal, vertical, horizontal, vertical]`**,
<br/>
or with a single value **`margin`**, which translates to **`[margin, margin, margin, margin]`**.
<br/>
<br/>
<h2 align="center">Fragment Shader (*.frag file)</h2>

<h3>multiplyRed.gre file:</h3>

```xml
<ShaderEffect name="Multiply Red">
    <Property name="red" type="float" min="0" max="1"
              ini="0" step="0.1" glValue="true"/>
</ShaderEffect>
```

<h3>multiplyRed.frag file:</h3>

```GLSL
// enve uses OpenGL 3.3
#version 330 core
// output color
layout(location = 0) out vec4 fragColor;

// processed texture coordinate, from the vertex shader
in vec2 texCoord;

// texture provided by enve
uniform sampler2D texture;
// the value for the 'red' property
uniform float red;

void main(void) {
    // texture pixel color at the coordinate
    vec4 color = texture2D(texture, texCoord);
    // assign value to the output color,
    // multiply red by the 'red' property value
    fragColor = vec4(color.r * red, color.g, color.b, color.a); 
}

```

<h3>The result:</h3>

![exampleRed](https://user-images.githubusercontent.com/16670651/75280663-29112f80-580e-11ea-901b-ef6a8df52a76.png#center)


If you are familiar with fragment shaders, the example above should be easy to understand.
<br/>
Enve provides a texture **`uniform sampler2D texture;`** and values for all Properties and Values with **`glValue="true"`** **`uniform float exampleFlotPropVal;`**, **`uniform int exampleIntPropVal;`**, **`uniform vec2 exampleVec2PropVal;`**, all you have to do is to calculate the result.
