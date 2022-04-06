#### Example
```
9 0 obj	% RichMedia annotation
<<	/Type /Annot
	/Subtype /RichMedia
	/AP << /N 10 0 R >>		% Appearance dictionary
	/BS				% Border Style dictionary
	<<	/Type /Border
		/W 0			% Width 0
	>>
	/F 68				% Flags
	/NM (PRC_Annotation)		% Annotation name
	/P 5 0 R			% Parent page
	/Rect [ 50 50 742 500 ]
	/RichMediaContent  12 0 R
	/RichMediaSettings 19 0 R
>>
endobj

10 0 obj	% Appearance dictionary
<<	/Length 39
	/Type /XObject
	/Subtype /Form
	/BBox [ 0.0 0.0 692 450 ]
	/Resources <<	
	    /XObject << /Im0 11 0 R >>
	>>
	/Matrix [ 1 0 0 1 0 0 ]
>>
stream 
q
  692 0 0 450 0 0 cm
  /Im0 Do
Q
endstream
endobj

11 0 obj	% Poster image for non-3D aware PDF processors
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

12 0 obj	% RichMediaContent dictionary
<<	/Type /RichMediaContent
	/Configurations [ 14 0 R ] 
	/Views [18 0 R]
	/Assets 21 0 R
>>
endobj

14 0 obj	% RichMediaConfiguration dictionary
<<	/Type /RichMediaConfiguration
	/Subtype /3D
	/Name (Default PRC configuration)
	/Instances [16 0 R]
>>
endobj

16 0 obj	% RichMediaInstance dictionary
<<	/Type /RichMediaInstance
	/Subtype /3D
	/Asset 23 0 R	% Reference to PRC filespec
>>
endobj

18 0 obj	% 3DView dictionary
<<	/Type /3DView
	/IN (12a345b6-7c89-0d1e-fa2b-cde3f45a6b78)	% internal name
	/XN (Default View)				% external name
	/MS /M						% Use C2W entry for transform matrix
	/NR true					% node return
	/RM << /Type /3DRenderMode /Subtype / SolidOutline >>
	/P << /Subtype /O >>				% orthographic projection
	/BG <</Subtype /SC /C [ 1 1 1 ] >>		% white background 
	/C2W [ -1.0 0.0 0.0 0.0 0.0 1.0 0.0 1.0 0.0 0.0 -10.0 0 ]
	<<	/Instance 2 0 R
		/Data (...State Data for 3D Instance...)
	>>
>>
endobj

19 0 obj	% RichMediaSettings Dictionary
<<	/Type /RichMediaSettings
	/Activation
	<<	/Type /RichMediaActivation
		/Condition /WhenClicked
		/Configuration 13 0 R	% Ref to element in Config array
		/View 18 0 R		% Ref to element in Views array
		/Presentation 20 0 R	% Ref to element in Presentation array
		/Scripts [22 0 R]	% Ref to JavaScript filespec	
	>>
	/Deactivation
	<<	/Type /RichMediaDeactivation
		/Condition /XD
	>>
>>
endobj

20 0 obj	% RichMediaPresentation Dict
<<	/NavigationPane true
	/Style /Embedded
	/PassContextClick false
	/Toolbar true
	/Type /RichMediaPresentation
>>
endobj

21 0 obj	% RichMediaAssets name tree
<<	/Names
	[ 
	  (script.js) 22 0 R
	  (3d.prc) 23 0 R
	]
>>
endobj

22 0 obj	% File spec dictionary for some JavaScript
<<	/Type /FileSpec
	/F (script.js)
	/UF (script.js)
	/EF << /F 24 0 R >>
>>
endobj

23 0 obj	% PRC file specification dictionary for the PRC 3D model
<<	/Type /FileSpec
	/F  (3d.prc)
	/UF (3d.prc)
	/EF << /F 25 0 R >>
>>
endobj

24 0 obj % embedded file stream for ECMAScript
<<	/Type /EmbeddedFile		% script.js
	/Subtype /text#2Fjavascript	% Registered MIME media type for ECMAScript (optional)
	/Length ...
	/Filter ...
>>
stream
% Data for script.js...
endstream
endobj


25 0 obj	% embedded file stream for 3D PRC data
<<	/Type /EmbeddedFile		% 3D.prc
	/Subtype /model#2Fprc		% Registered MIME media type (optional)
	/Length ...
	/Filter ...
>>
stream
% Data for 3D.prc ...
endstream
endobj
```
