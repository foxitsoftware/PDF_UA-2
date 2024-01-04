# PDF_UA-2
ISO 14289-2 (PDF/UA-2) significantly improves the accessibility of PDF. Foxit's PoC implementation of various PDF creation and consumption tools

We provide [Installer](installer/README.md) to our PoC implementations.
The [PDF Sample files](samples/README.md) we produced to achieve interoperability

We are testing several AI approaches to help with automatic math recognition as a part of remediation processes. The results of our discoveries can be found here:
[Recognition](recognition/Readme.md)

If you are interested in interoperable testing, have comments, or want to report a bug or suggestion, please use the Issues feature on github.

# PDF 2.0 and PDF/UA-2 features

The ISO 32000-2:2020 (PDF 2.0) specification only provides file format requirements. It should give clear guidance for PDF creation tools. What should and should not be present in the compliant file, but is not clear how the processor should use all those information. We use this github repository to explain our reasoning with implementations and sample files so we can reach a consensus with other developers. It is essential to give end users the same experience when consuming PDF UA-2 files through AT tools.

The following sections explain our implementation decisions. Feel free to comment.

## MathML

PDF 2.0 in conjunction with PDF/UA-2 recognizes two methods of semantically denote math. Both methods require the use of *Formula* structure element and then:
- use \<mathml\> elements in MathML namespace as direct child 
- or the associate MathML data in *AF* property of structure element

In our PoC we use the second option.  
 

## Logic in providing Associated file

PDF 2.0 doesn't provide any guidance when it comes to processors, so it's not specified how a Reader should deal with specific scenarios. 

Our algorithm when processing PDF files with *AF* is as follows:

For *Formula* structure element:
- Search only AFs with *AFRelationship* == *Supplement* (as per PDF/UA-2)
- accepting *AF* as a directory as well as an array (PDF 2.0 only allows arrays, but we noticed files with AF as a dictionary)
- if no *AF* entry with *Supplement* exists, we are processing kids of structure element
- ignoring the media type of the associated file (todo)
- if we find suitable *AF*, then substructure is ignored and the content of stream data is provided as a textual value

### Known issues
- Substructure is ignored if AF exists, therefore *Lbl* structure element with it's content might get lost

## *RoleMapNS*
Currently, we only check for *RoleMapNS* if the document is marked as 2.0 (header must be 2.0 !)
Our tools don't check namespaces. 
If the file is 2.0, then *RoleMapNS* is the preferred method, no matter what structure element we encounter.

## Use of *ActualText* property
If *ActualText* is present we ignore AFs and substructure. *ActualText* serves as a full replacement of structure element (this is true for all structure elements. **Formula** is no different)



