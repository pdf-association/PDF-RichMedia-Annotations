# Introduction

This document provides background to the dictionaries and other entries that define 3D content in PDF 2.0. It is written for software developers want to create PDF 2.0 files with 3D RichMedia Annotations.

# PDF 2.0

## PDF Version

Section 7.5.2, "File header" of ISO 32000-2 specifies that a conforming document "shall identify the version (either in the header or as the value of the Version entry in the document's catalog dictionary) as 2.0."

The recommended practice is to identify the version in the file header using the string **%PDF--2.0**.

# RichMedia Annotations

## Embedded file streams

The recommended practice is to store the PRC and JavaScript data as embedded file streams (ISO 32000-2, 7.11.4). This ensures that the PDF file is self-contained and can be shared as a single file.

It is recommended that embedded file streams include the following entries:

-   **Length**
    Required for all stream dictionaries.

-   **Subtype**
    PRC embedded file streams should include a **Subtype** entry of *model/prc*
    U3D embedded file streams should include a **Subtype** entry of *model/u3d*
    JavaScript embedded file streams should include a **Subtype** entry of *text/javascript*

![](./images/PDF/FileSpecificationDictionary.png)

Figure 1 - Embedded file streams

#### Example
```
25 0 obj	% embedded file stream for 3D PRC data
<<	/Type /EmbeddedFile		% 3D.prc
	/Subtype (model/prc)
	/Length ...
>>
stream
% Data for 3D.prc ...
endstream
endobj

24 0 obj	% embedded file stream for JavaScript
<<	/Type /EmbeddedFile		% script.js
	/Subtype (text/javascript)
	/Length ...
	/Filter ...
>>
stream
% Data for script.js...
endstream
endobj
```
## File specification dictionary

The recommended practice is to create file specification dictionaries (ISO 32000-2, 7.11.3) for all the embedded file streams associated with the 3D RichMedia annotation (PRC and JavaScript files).

It is recommended that file specification dictionaries include the following entries:

-   **EF**
    Required for all file specification dictionaries. Must be an indirect reference to an embedded file stream.

-   **F**
    It is recommended that the **F** entry is the same as the filename of the embedded file stream.

-   **UF**
    It is recommended that the **UF** entry contains a Unicode version of the filename of the embedded file stead (**F** key).

-   **Type**
    Required by the inclusion of the **EF** key. Must be "*FileSpec*".

![](images/PDF/FileSpecificationDictionary.png)

Figure 2 - File specification dictionary

#### Example
```
23 0 obj	% PRC file specification dictionary
<<	/Type /FileSpec
	/F (3d.prc)
	/UF (3d.prc)
	/EF  /F 25 0 R >>
>>
endobj

22 0 obj	% JavaScript File spec dictionary
<<	/Type /FileSpec
	/F (script.js)
	/UF (script.js)
	/EF << /F 24 0 R >>
>>
endobj
```
## Assets name tree

The recommended practice is to create a name tree (ISO 32000-2, 7.11.3) for the PRC and JavaScript file specification dictionaries.

![](images/PDF/AssetsNameTree.png)

Figure 3 - Assets name tree

#### Example
```
21 0 obj	% RichMediaAssets name tree
<<	/Names
	[ (3d.prc) 23 0 R
	  (script.js) 22 0 R
	]
>>
endobj
```

## RichMediaPresentation dictionary

The recommended practice is to create a RichMediaPresentation dictionary (ISO 32000-2, 13.7.2.2.5) to specify how the user interface should be presented.

![](images/PDF/RichMediaPresentationDictionary.png)

Figure 4 - RichMediaPresentation dictionary

#### Example
```
20 0 obj	% RichMediaPresentation Dict
<<	/NavigationPane true
	/Style /Embedded
	/PassContextClick false
	/Toolbar true
	/Type /RichMediaPresentation
>>
endobj
```

## RichMediaActivation dictionary

![](images/PDF/RichMediaActivationDictionary.png)

Figure  5- RichMediaActivation dictionary

## RichMediaDeactivation dictionary

![](images/PDF/RichMediaDeactivationDictionary.png)

Figure 6 - RichMediaDeactivation dictionary

## RichMediaSettings dictionary

![](images/PDF/RichMediaSettingsDictionary.png)

Figure 7 - RichMediaSettings dictionary

#### Example
```
19 0 obj	% RichMediaSettings Dictionary
<<	/Type /RichMediaSettings
	/Activation
	<<	/Type /RichMediaActivation
		/Condition /WhenClicked
		/Configuration 13 0 R	% Ref to element in Config array
		/View 18 0 R			% Ref to element in Views array
		/Presentation 20 0 R	% Ref to element in Presentation array
		/Scripts				% Ref to JavaScript filespec
		[22 0 R]
	>>
	/Deactivation
	<<	/Type /RichMediaDeactivation
		/Condition /XD
	>>
>>
endobj
```

## View dictionary

![](images/PDF/3DViewDictionary.png)

Figure 8 - 3DView dictionary

#### Example
```
18 0 obj	% 3DView dictionary
<<	/Type /3DView
	/IN (12a345b6-7c89-0d1e-fa2b-cde3f45a6b78)	% internal name
	/XN (Default View) 							% external name
	/MS /M										% Use C2W entry for transform matrix
	/NR true									% node return
	/RM << /Type /3DRenderMode /Subtype / SolidOutline >>
	/P << /Subtype /0 >>						% orthographic projection
	/BG <</Subtype /SC /C [ 1 1 1 ] >>			% white background
	/C2W [ -1.0 0.0 0.0 0.0 0.0 1.0 0.0 1.0 0.0 0.0 -10.0 0 ]
	<<	/Instance 2 0 R
		/Data (...State Data for 3D Instance...)
	>>
>>
endobj
```

## View array

#### Example
```
17 0 obj	% Views array
	[18 0 R]
endobj
```

## RichMediaInstance dictionary

![](images/PDF/RichMediaInstanceDictionary.png)

Figure 9 - RichMediaInstance dictionary

#### Example
```
16 0 obj	% RichMediaInstance dictionary
<<	/Type /RichMediaInstance
	/Subtype /3D
	/Asset 23 0 R	% Reference to PRC filespec
>>
endobj
```

## RichMediaInstance array

#### Example
```
15 0 obj	% RichMediaInstance array
	[16 0 0 obj	% Ref to RichMediaInstance dictionary
    ]
endobj

```

## RichMediaConfiguration dictionary

![](images/PDF/RichMediaConfigurationDictionary.png)

Figure 10 - RichMediaConfiguration dictionary
#### Example
```
14 0 obj	% RichMediaContent dictionary
<<	/Type /RichMediaContent
	/Configurations 13 0 R
	/Views 17 0 R
	/Assets 21 0 R
>>
endobj
```

## RichMediaConfiguration array

#### Example
```
13 0 obj % RichMediaConfiguration array
	[14 0 R
	]
endobj
```

## RichMediaContent dictionary

![](images/PDF/RichMediaContentDictionary.png)

Figure 11 - RichMediaContent dictionary

#### Example
```
12 0 obj	% RichMediaContent dictionary
<<	/Type /RichMediaContent
	/Configurations 13 0 R
	/Views 17 0 R
	/Assets 21 0 R
>>
endobj
```
## Poster image stream

#### Example
```
11 0 obj	% Poster image
<<	/Length ...
	/Width ...
	/Height ...
	/BitsPerComponent 8
	/ColorSpace /DeviceRGB
	/Subtype /Image
>>
stream
...Poster Image Stream...
endstream
endobj
```
## Appearance dictionary

![](images/PDF/AppearanceDictionary.png)

Figure 12 - Appearance dictionary

#### Example
```
10 0 obj	% Appearance dictionary
<<	/Length 39
	/Type /XObject
	/BBox [ 0.0 0.0 692 450 ]
	/Resources
	<<	/XObject << /Im0 11 0 R >>
		/ProcSet [ /PDF /ImageC ]
		/ExtGState <</GS0 << /Type /ExtGState /ca 1.0 /CA 1.0 >> >>
	>>
	/Subtype /Form
	/Matrix [ 1 0 0 1 0 0 ]
>>
stream q
/GS0 gs
692 0 0 450 0 0 cm
/Im0 Do
Q
endstream
endobj

```

## RichMedia annotation

![](images/PDF/RichMediaAnnotation.png)

Figure 13 - RichMedia annotation dictionary

#### Example
```
9 0 obj	% RichMedia annotation
<<	/Type /Annot
	/Subtype /RichMedia
	/NM (PRC_Annotation)	% Annotation name
	/AP << /N 10 0 R >>	% Appearance dictionary
	/BS	% Border Style dictionary
	<<	/Type /Border
		/W 0	% Width 0
	>>
	/F 68	% Flags
	/P 5 0 R	% Parent
	/Rect [ 50 50 742 500 ]
	/RichMediaContent 12 0 R
	/RichMediaSettings 19 0 R
>>
endobj

```
