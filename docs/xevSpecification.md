<h1 align="center">Xev File Format Specification</h1>

Xev is a XML-based format similar to ora and kra.</br>
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
