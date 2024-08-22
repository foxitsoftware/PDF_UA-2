# PDF/UA-2
ISO 14289-2 (PDF/UA-2) significantly improves the accessibility of PDF. Foxit's PoC implementation of various PDF creation and consumption tools

We provide [Installer](installer/README.md) for our PoC implementations.
The [PDF Sample files](samples/README.md) we produced to achieve interoperability

We are testing several AI approaches to help with automatic math recognition as a part of remediation processes. The results of our discoveries can be found here:
[Recognition](recognition/Readme.md)

If you are interested in interoperable testing, have comments, or want to report a bug or suggestion, please use the Issues feature on GitHub.

# PDF 2.0 and PDF/UA-2 features

The ISO 32000-2:2020 (PDF 2.0) specification only provides file format requirements. It should give clear guidance for PDF creation tools. What should and should not be present in the compliant file, but it is not clear how the processor should use all that information. We use this GitHub repository to explain our reasoning with implementations and sample files so we can reach a consensus with other developers. It is essential to give end users the same experience when consuming PDF UA-2 files through AT tools.

The following sections explain our implementation decisions. Feel free to comment.

## MathML

PDF 2.0 in conjunction with PDF/UA-2 recognizes two methods of semantically denote math. Both methods require the use of *Formula* structure element and then:
- Use \<mathml\> elements in the MathML namespace as direct children 
- or the associated MathML data in the *AF* property of the structure element

In our PoC we use the second option.  
 
## Logic in providing Associated file

PDF 2.0 doesn't provide any guidance regarding processors, so it's not specified how a processor should deal with specific scenarios. 

Our algorithm when processing PDF files with *AF* is as follows:

For *Formula* structure element:
1. **Search Criteria:**
   - Focus only on AFs where `AFRelationship == Supplement` (as per PDF/UA-2).
   - Ensure that the `Mediatype` (Subtype key) is `application/mathml+xml`.
     - *Note:* The Mediatype comparison is currently a string match, with ongoing discussions in the community about potential variations in the key.

2. **AF Structure Handling:**
   - Accept both arrays and directories for AF. While PDF 2.0 officially allows only arrays, some files use AF as a dictionary.
   - - Only the first element that meets the criteria is used

3. **Fallback Mechanism:**
   - If no suitable AF entry is found proceed to process the children (kids) of the structure element.
   - If Formula doesn’t have NS == 2.0 then the old processing is used.

4. **Unimplemented Features:**
   - `AFRelationship == Alternative` is not yet implemented. The usage of this key is currently under discussion.

5. **Priority Rules:**
   - `ActualText` takes precedence over the processing of the structure element itself.
   - `AF` has priority over `Alt`.

6. **Substructure Handling:**
   - If a suitable `AF` is found, process the substructure of the element and provide the content of the stream data as the textual value, i.e. only that content is replaced, not the substructure.
   - If `ActualText` is present we ignore AFs and substructure. `ActualText` serves as a full replacement of structure element (this is true for all structure elements. `Formula` is no different)

7. **Processing Formula Pseudocode**
```pseudo
IF ActualText is present THEN
    // Ignore AFs and substructures, and provide ActualText
    Provide ActualText

ELSE IF AF is present AND Namespace == 2.0 AND AFRelationship == Supplement AND Mediatype == application/mathml+xml THEN 
   // Only the first element that meets the criteria is used
   // Process the substructure and provide the stream data content as textual value
   Provide stream data content, replacing content but not substructure
   
ELSE IF Alt is present THEN
    // Provide Alt
    Provide Alt

ELSE
    // Process the substructure of the element
    Proved content and process substructure
```

## *Rolemap and RolemapNS Processing*

1. **General Rolemap Handling:**
   - Process the `Rolemap` or `RolemapNS` on a Structure Element (SE) in a PDF, regardless of the PDF version.

2. **Namespace (NS) Check:**
   - If the SE does **not** have a Namespace (NS), check the `Rolemap`.
   - If the SE **does** have an NS and it is other than `PDF 1.7`, `PDF 2.0`, or `MathML`, check `RolemapNS`.

3. **Custom Element Mapping:**
   - Allow mapping of a single custom element into two different standard elements based on `RolemapNS`.

4. **Backward Compatibility:**
   - If the *Formula* element does not have `NS == 2.0`, then revert to the old processing method. This is our backward-compatible hack since older processors won’t check the NS.

5. **Namespace Flexibility:**
   - Elements could be in any namespace (e.g., LaTeX). The discussion of `NS == 2.0` occurs after rolemapping (i.e., `RolemapNS`).
