# Frequently Asked Questions

## Where is the classes RTView 
The class `RTView` belongs to [Roassal2](https://github.com/ObjectProfile/Roassal2). A class that begins with `RT` belongs to Roassal2. In Roassal3, classes begins with `RS`. 

## What about the book Agile Visualization
The book [http://agilevisualization.com](http://agilevisualization.com) is about Roassal2. Very little can be reused from the book for Roassal3. A new edition of Agile Visualization will appear during 2020.

## How to export a visualization to PDF or SVG?
We provide a set of exporter in https://github.com/ObjectProfile/Roassal3Exporters
We support PDF, SVG, MOV, and MP4. The last two file format are useful for animations.

## How to export a visualization in a given size of a PDF?
You need to have the exporters https://github.com/ObjectProfile/Roassal3Exporters
The following code exports a visualization as a PDF file, using letter size:

```Smalltalk
c := RSCanvas new.

lbls := RSGroup new.
Collection withAllSubclassesDo: [ :cls |
    lbl := RSLabel new text: cls name; model: cls.
    lbls add: lbl ].
c addAll: lbls.

RSNormalizer fontSize
    shapes: lbls;
    normalize: #numberOfMethods.
   
c @ RSHierarchyPacker.

c extent: (RSPageExtent letter).
c when: RSExtentChangedEvent do: [ :evt |
	c camera zoomToFit: c extent.
	].

c pdfExporter
	fileName: '/tmp/test3.pdf';
	export.
```

## How to make an object visualizable using GTInspector?
GTInspector is a moldable inspector that is part of Pharo and was developed as part of the Glamorous Toolkit project. GTInspector offers a way to hook visualization in it. 

```Smalltalk
MyClass>>gtInspectorViewIn: composite
	<gtInspectorPresentationOrder: -10>
	composite roassal3
		title: 'View';
		initializeCanvas: [ | c |
			c := RSCanvas new.
			...
			c ]
			
```
It is a good practice to have `initializeCanvas: [ self visualize ]` and define a method `visualize` in `MyClass`.
Either way, you can now inspect an instance of `MyClass`.
