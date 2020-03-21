# PDF.js - Overall Architecture Revisited

PDF.js is a Portable Document Format(PDF) viewer that is built with HTML5, CSS and javascript. It is a community driven project with the main goal of being a general-purpose, web standards-based platform for parsing and rendering pdf files. It is directly built into Firefox version 19+ and it can be downloaded as an extension on browsers such as Google Chrome.

PDF.js’s overall architecture is made up of different layers and understanding each layer is key to understanding the overall project. PDF.js is split into three main layers, namely the Core, Display, and Viewer layer.

<div align="center">
    <img src="../Images/LayeredArchitecture.svg"/>
</div>

## Higher-level Diagram

## Core [(src/core)](https://github.com/CSCD01-team32/pdf.js/blob/af8d0b9597ccd0e020910eafd74dd6ad140db520/src/core)

The core layer is the layer where a PDF in binary is parsed and interpreted. It is the foundation for all subsequent layers. It is located in the src/core folder. It has different files for interpreting and parsing PDF files.

## Display [(src/display)](https://github.com/CSCD01-team32/pdf.js/blob/af8d0b9597ccd0e020910eafd74dd6ad140db520/src/display)

The display layer takes the core layer and exposes an easier to use API to render PDFs and get other information out of a document. The display layer can be found in the src/display folder.

The main file in the display layer is the file called api.js. In this file there are classes that deal with rendering and proxy of a PDF document. A description of the classes can be seen in the table below.

| **Class** | **Description** |
|-------|-------------|
|PDFDocumentLoadingTask       |Controls the operations required to load a PDF document (such as network requests) and provides a way to listen for completion, after which individual pages can be rendered             |
|PDFDataRangeTransport       |Supports range requests             |
|PDFDocumentProxy       |A proxy to a PDFDocument|
|PDFPageProxy       |A proxy to a PDFPage in the worker thread             |
|PDFWorker       |PDF.js web worker abstraction, which controls the instantiation of PDF documents. Message handlers are used to pass information from the main thread to the worker thread and vice versa. If the creation of a web worker is not possible, a "fake" worker will be used instead.             |
|RenderTask       |Allows for control of the rendering tasks             |
|InternalRenderTask       |Internally used by RenderTask class             |

## Viewer [(web)](https://github.com/CSCD01-team32/pdf.js/blob/af8d0b9597ccd0e020910eafd74dd6ad140db520/web)

The viewer is built on the display layer and is the User Interface (UI) for PDF viewer in Firefox and the other browser extensions within the project. The viewer layer can be found in the web folder.

## External [(external)](https://github.com/CSCD01-team32/pdf.js/blob/af8d0b9597ccd0e020910eafd74dd6ad140db520/external)

The external folder contains third party code that the system interacts with.

## Design Patterns/Principles

### Factory Design Pattern

The Factory design pattern is a recurring pattern used for different classes in this project. For example, the file annotation.js evidently uses the factory design pattern (see image below) for the creation of different types of annotation representations for a pdf.

<div align="center">
    <img src="../Images/Annotation.png"/>
</div>

In the image above, the class Annotation factory creates any type of annotation.

## Conclusion

The overall system is well designed, but there is a lot of code that is not well documented, so reading through some of the code and trying to understand what was going on was pretty difficult. We like how they used a shared folder for code that is used by both the core and display layer.

We liked how the project is split into layers, each layer having a specific role in the overall structure of the system, rather than just putting everything in one big folder.

We found it interesting that there were several ways to run the project. In the viewer layer there is a file called pdf.js that packages the project and creates a prebuilt version of the project that includes a generic build of PDF.js and the viewer, rather than just directly running it from the source code or downloading it as an extension for browsers like Google Chrome.
