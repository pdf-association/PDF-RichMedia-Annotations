# Introduction

This document provides background to the dictionaries and other entries that define RichMedia content in PDF 2.0, including 3D. It is written for software developers wanting to create PDF 2.0 files with RichMedia Annotations.

# PDF 2.0

## PDF Version

Section 7.5.2, "File header" of ISO 32000-2 specifies that a conforming document "shall identify the version (either in the header or as the value of the **Version** entry in the document's catalog dictionary) as 2.0". The recommended practice is to identify the version in the file header using the string `%PDF-2.0`.

# RichMedia Annotations

## Embedded file streams

The recommended practice is to store the RichMedia assets such as PRC and JavaScript data as embedded file streams (ISO 32000-2, 7.11.4). This ensures that the PDF file is self-contained and can be shared as a single file. ISO 32000-2:2020 requires that the `Subtype` key (a PDF name object) is a valid MIME media type as defined in Internet RFC 2046, with the provision that characters not permitted in names shall use the 2-character hexadecimal code format.

It is recommended that embedded file streams include the following entries:

-   **Length**
    Required for all stream dictionaries.

-   **Subtype**
    - PRC embedded file streams should include a `Subtype` entry of `model#2Fprc` which is *model/prc* encoded as a PDF name where `/` becomes `#2F` (as [registered with IANA](https://www.iana.org/assignments/media-types/media-types.xhtml#model))
    - U3D embedded file streams should include a `Subtype` entry of `model#2Fu3d` which is *model/u3d* encoded as a PDF name where `/` becomes `#2F` (as [registered with IANA](https://www.iana.org/assignments/media-types/media-types.xhtml#model))
    - STEP AP 242 embedded file streams should include a `Subtype` entry of `model#step`, `model#step+xml`, `model#step+zip` or `model#step-xml+zip` (as [registered with IANA](https://www.iana.org/assignments/media-types/media-types.xhtml#model))
    - JavaScript embedded file streams should include a `Subtype` entry of `text#2Fjavascript` which is *text/javascript* encoded as a PDF name where `/` becomes `#2F` (as [registered with IANA](https://www.iana.org/assignments/media-types/media-types.xhtml#text))

### Example
```
25 0 obj	% embedded file stream for 3D PRC data
<<	/Type /EmbeddedFile		% 3D.prc
	/Subtype /model#2Fprc           % required to be a PDF name - value is "model/prc" encoded as a PDF name
	/Length ...
>>
stream
% Data for 3D.prc ...
endstream
endobj

24 0 obj	% embedded file stream for JavaScript
<<	/Type /EmbeddedFile		% script.js
	/Subtype /text#2Fjavascript     % required to be a PDF name - value is "text/javascript" encoded as a PDF name
	/Length ...
	/Filter ...
>>
stream
% Data for script.js...
endstream
endobj
```
## File specification dictionary

The recommended practice is to create file specification dictionaries (ISO 32000-2, 7.11.3) for all the embedded file streams associated with the 3D RichMedia annotation (e.g. PRC and JavaScript files).

It is recommended that file specification dictionaries include the following entries:

-   **EF**
    Required for all file specification dictionaries. Must be an indirect reference to an embedded file stream. Both `F` and `UF` should be present as recommended in ISO 32000-2:2020 Table 43, `EF` key description.

-   **F**: it is recommended that the `F` entry is the same as the filename of the embedded file stream.

-   **UF**: it is recommended that the `UF` entry contains a Unicode version of the filename of the embedded file (`F` key). For simple ASCII filenames, this can be the same object as for the `F' key.

-   **Type**: required by the inclusion of the `EF` key. Must be "*FileSpec*".

### Example
```
23 0 obj	% PRC file specification dictionary
<<	/Type /FileSpec
	/F (3d.prc)
	/UF (3d.prc)
	/EF << /F 25 0 R /UF 25 0 R >>
>>
endobj

22 0 obj	% JavaScript File spec dictionary
<<	/Type /FileSpec
	/F (script.js)
	/UF (script.js)
	/EF << /F 24 0 R /UF 24 0 R >>
>>
endobj
```
## Assets name tree

The recommended practice is to create a name tree (ISO 32000-2, 7.11.3) for the PRC and JavaScript file specification dictionaries.

### Example
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

## RichMediaSettings dictionary

This dictionary includes the `Activation` and `Deactivation` dictionaries.

The `Scripts` key was accidentally omitted from Table 335 in ISO 32000-2:2020 and has since been rectified in the [PDF 2.0 errata](- see https://pdf-issues.pdfa.org/32000-2-2020/clause13.html#Table335).

### Example
```
19 0 obj	% RichMediaSettings Dictionary
<<	/Type /RichMediaSettings
	/Activation
	<<	/Type /RichMediaActivation
		/Condition /XA          % explicit action
		/Configuration 13 0 R	% Ref to element in Config array
		/Views [ 18 0 R ]       % Ref to 3D view dict element in Views array
		/Presentation 20 0 R	% Ref to element in Presentation array
		/Scripts [ 22 0 R ]     % Reference to JavaScript filespec. Corrected in errata 
	>>
	/Deactivation
	<<	/Type /RichMediaDeactivation
		/Condition /XD          % explicit deactivation
	>>
>>
endobj
```

## View dictionary

### Example
```
18 0 obj	% 3DView dictionary
<<	/Type /3DView
	/IN (12a345b6-7c89-0d1e-fa2b-cde3f45a6b78)      % internal name
	/XN (Default View)                              % external name
	/MS /M                                          % Use C2W entry for 12 element transform matrix
	/C2W [ -1.0 0.0 0.0 0.0 0.0 1.0 0.0 1.0 0.0 0.0 -10.0 0 ]
	/NR true                                        % node return
	/RM << /Type /3DRenderMode /Subtype / SolidOutline >> % render mode dictionary: black (default AC key) solid outline
	/P << /Subtype /O >>                            % Projection dictionary: orthographic projection
	/BG << /Type /3DBG /Subtype /SC /C [ 1 1 1 ] >> % white background
>>
endobj
```

## RichMediaInstance dictionary

### Example
```
16 0 obj	% RichMediaInstance dictionary
<<	/Type /RichMediaInstance
	/Subtype /3D
	/Asset 23 0 R	 % Reference to PRC filespec, also referenced from Assets name tree in RichMediaContent
>>
endobj
```

## RichMediaInstance array

### Example
```
15 0 obj	% RichMediaInstance array
    [ 16 0 R ]  % Ref to RichMediaInstance dictionary
endobj
```

## RichMediaConfiguration dictionary

### Example
```
14 0 obj	% RichMediaContent dictionary
<<	/Type /RichMediaContent
	/Configurations [ 14 0 R ]     % RichMediaConfiguration array
	/Views 17 0 R
	/Assets 21 0 R
>>
endobj
```

## RichMediaContent dictionary

### Example
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

### Example
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
%...Poster Image Stream...
endstream
endobj
```
## Appearance dictionary

### Example
```
10 0 obj	% Appearance dictionary
<<	/Subtype /Form
        /Length 39
	/Type /XObject
	/BBox [ 0.0 0.0 692 450 ]
	/Resources
	<<	/XObject << /Im0 11 0 R >>
		/ExtGState << /GS0 << /Type /ExtGState /ca 1.0 /CA 1.0 >> >>
	>>
	/Matrix [ 1 0 0 1 0 0 ]
>>
stream 
  q % push graphics state
    /GS0 gs % set graphics state
    692 0 0 450 0 0 cm % set matrix
    /Im0 Do % paint the poster image
  Q % pop graphics state
endstream
endobj
```

## RichMedia annotation

### Example
```
9 0 obj	% RichMedia annotation
<<	/Type /Annot
	/Subtype /RichMedia
	/NM (PRC_Annotation)	% Annotation name
	/AP << /N 10 0 R >>	% Appearance dictionary
	/BS <<	% Border Style dictionary
		/Type /Border
		/W 0	% Width 0
	>>
	/F 68	        % Flags
	/P 5 0 R	% Parent
	/Rect [ 50 50 742 500 ]
	/RichMediaContent 12 0 R
	/RichMediaSettings 19 0 R
>>
endobj
```
