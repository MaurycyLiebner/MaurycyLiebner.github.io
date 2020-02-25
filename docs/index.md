<br/>
<h1 align="center">Shader Effects</h1>
<br/>

Shader effects let users create their own raster effects. You need two files to get started with shader effects.<br/>

* The <a href="https://www.w3schools.com/xml/xml_whatis.asp">XML</a> <b>*.gre</b> file defines the properties for the effect.
* The <a href="https://www.khronos.org/opengl/wiki/Fragment_Shader">GLSL fragment shader</a> <b>*.frag</b> file defines the effect for a given set of property values.

To use shader effects, both <b>\*.gre</b> and <b>\*.frag</b> files have to be placed in **`home_directory/.enve/ShaderEffects`** directory.</br>
The <b>\*.gre</b> and <b>\*.frag</b> files have to share a name, e.g., **`exampleEffect.gre`** and **`exampleEffect.frag`**.
<br/>

<p align="center"><a href="https://github.com/MaurycyLiebner/enve/tree/master/examples/shaderEffects">Here you can find modest examples</a></p>
<h2 align="center">Properties Definition (*.gre file)</h2>

<h3>Root Element</h3>

```xml
<ShaderEffect name="Example Effect">
  <!-- properties and values -->
</ShaderEffect>
```

A **ShaderEffect** <a href="https://en.wikipedia.org/wiki/Root_element">root element</a> has only a **name** attribute. The **name** will be visible in enve interface.</br>
The **ShaderEffect** encloses all properties and values for the effect.

<h3>Properties</h3>

There are three types of properties:
* **int** - integer properties
* **float** - floating-point number properties
* **vec2** - two-value float properties

```xml
<ShaderEffect name="Example Effect">
    <Property name="exampleProperty" nameUI="example property"
              type="float" min="0" max="100" ini="8" step="1"
              glValue="true" resolutionScaled="true"/>
</ShaderEffect>
```

* **name** - name of that will be used to reference the property, cannot contain spaces or special characters, e.g., **`exampleProperty`**
* **nameUI** - name of the property that will be visible in enve interface, e.g., **`example property`**
* **xnameUI** - name of the x comonent of **vec2**
* **ynameUI** - name of the y comonent of **vec2**
* **type** - type of the property, e.g., **`int`**, **`float`**, **`vec2`**
* **min** - minimum value that can be assigned to the property, e.g., **`0`**, **`[0, 0]`**
* **max** - maximum value that can be assigned to the property, e.g., **`100`**, **`[100, 50]`**
* **ini** - initial value for the property, e.g., **`55`**, **`[75, 25]`**
* **step** - recommended value increment for user interactions with the interface, e.g., **`1`**, **`[1, 1]`**
* **glValue** - specifies whether the property is used in the fragment shader, e.g., **`true`**, **`false`**
* **resolutionScaled** - specifies whether the property value should be multiplied by scene resolution, e.g., **`true`**, **`false`**

As you might have guessed, the **`[x, y]`** attribute values are only supported by **vec2** type, but they are not mandatory, **vec2** also accepts **`x`**, that will automatically be expanded to **`[x, x]`**.

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

<br/>
