## **math-tag (2.0).pdf**
- **PDF Header:** 2.0
- **Namespace on Structure Elements:** 2.0
- **AF on Formula:** With `AFRelationship == Alternative` (against PDF/UA-2 standards)
- **Namespace:** LaTeX
- **RoleMapNS:** Used

## **Variance - Wikipedia - Tagged - AF - Ref(OBJR) - NS.pdf**
- **PDF Header:** 2.0
- **Namespace on Structure Elements:** 2.0
- **AF on Formula:** With `AFRelationship == Supplement`
- **First Formula:** Contains `Lbl` (Reference to `FENote`)
- **FENote:** Contains Reference

## **No Lbl Variance - Wikipedia - Tagged - AF - Ref(OBJR) - NS.pdf**
- **PDF Header:** 1.6
- **Namespace on Structure Elements:** 2.0
- **AF on Formula:** With `AFRelationship == Supplement`
- **First Formula:** Without `Lbl` (works)
- **FENote:** Contains Reference

## **Sample AF Math(1.6).pdf**
- **PDF Header:** 1.6
- **Namespaceon Structure Elements:** None
- **AF on Formula:** As a dictionary (invalid structure)
- **AFRelationship:** Supplement

## mathml-AF-complex test.pdf
### Each Formula is Supposed to Contain a Specific Problem:
(It is also important to mention that all the techniques are fully compatible with PDF/UA-2, Well Tagged PDF and ready for Derivation to HTML)

1. **Two AFs, One with Source, other with Supplement and Media Type is in Mixed Upper and Lowercase `APPLICATION/mathml+xml`. No Alt present:**
   - **Formula:** 𝑎𝑥2 + 𝑏𝑥 + 𝑐 = 0
   - **AFs:**
   - - **1.**
   - - - **Relationship:** Supplement
   - - - **Subtype:** APPLICATION/mathml+xml
   - - **2.**
   - - - **Relationship:** Source
   - - - **Subtype:** application/x-tex
   - **Expected Result:** Should return the content of the first AF in the array. The media type is case insensitive.
   - - **Value:** \<math\> \<mi\>&#x1d44e;\</mi\> \<mo\>&#x2062;\</mo\> \<msup\> \<mi\>&#x1d465;\</mi\> \<mn\>2\</mn\> \</msup\> \<mo\>+\</mo\> \<mi\>&#x1d44f;\</mi\> \<mo\>&#x2062;\</mo\> \<mi\>&#x1d465;\</mi\> \<mo\>+\</mo\> \<mi\>&#x1d450;\</mi\> \<mo\>=\</mo\> \<mn\>0\</mn\> \</math\>
2. **Two AFs. One with Source, other with Supplement. The Source with media type `application/mathml+xml` and Supplement with `application/x-tex`. No Alt present.**
   - **Formula:** 𝑥=−𝑏±√𝑏2−4𝑎𝑐2𝑎
   - **AFs:**
   - - **1.**
   - - - **Relationship:** Source
   - - - **Subtype:** application/mathml+xml
   - - **2.**
   - - - **Relationship:** Supplement 
   - - - **Subtype:** application/x-tex
   - **Expected Result:**  No AF meets criteria. No Alt present. It should return the content items under Formula structure element.
   - - **Value:** 𝑥=−𝑏±√𝑏2−4𝑎𝑐2𝑎
3. **Two AFs. One with Source, other with Supplement. Both don't have the correct media type. No Alt present.**
   - **Formula:** |−1|=1
   - **AFs:**
   - - **1.**
   - - - **Relationship:** Supplement
   - - - **Subtype:** WRONG_MEDIA
   - - **2.**
   - - - **Relationship:** Source
   - - - **Subtype:** application/x-tex
   - **Expected Result:** No AF meets criteria. No Alt present. It should return the content items under Formula structure element.
   - - **Value:** |−1|=1
4. **Two AFs. One with Source, other with Alternative. Alternative with the correct media type. No Alt present.**
   - **Formula:** (1234)(1101)=(1337)
   - **AFs:**
   - - **1.**
   - - - **Relationship:** Alternative
   - - - **Subtype:** application/mathml+xml
   - - **2.**
   - - - **Relationship:** Source
   - - - **Subtype:** application/x-tex
   - **Expected Result:** Alternative is not defined yet.
   - **Value:** (1234)(1101)=(1337)
5. **Two AFs. Both meet criteria. Alt Present.**
   - **Formula:** sin2 𝜃 + cos2 𝜃 = 1
   - **AFs:**
   - - **1.**
   - - - **Relationship:** Supplement
   - - - **Subtype:** application/mathml+xml
   - - **2.**
   - - - **Relationship:** Supplement
   - - - **Subtype:** application/mathml+xml
   - **Alt:** Alternate text
   - **Expected Result:** Just the content of the first AF should be exposed, Alt is ignored.
   - - **Value:** \<math display="block"\> \<msup\> \<mi mathvariant="normal"\>sin\</mi\> \<mn\>2\</mn\> \</msup\> \<mo rspace="0.167em"\>&#x2061;\</mo\> \<mi\>&#x1d703;\</mi\> \<mo\>+\</mo\> \<msup\> \<mi mathvariant="normal"\>cos\</mi\> \<mn\>2\</mn\> \</msup\> \<mo rspace="0.167em"\>&#x2061;\</mo\> \<mi\>&#x1d703;\</mi\> \<mo\>=\</mo\> \<mn\>1\</mn\> \</math\>
6. **Two AFs. None meet criteria, Alt present.**
   - **Formula:** 2𝑥 + 𝑦 = 3 𝑥 − 𝑦 = 0
   - **AFs:**
   - - **1.**
   - - - **Relationship:**
   - - - **Subtype:** application/mathml+xml
   - - **2.**
   - - - **Relationship:** Source
   - - - **Subtype:** application/x-tex
   - **Alt:** Alternate
   - **Expected Result:** If no associated file meets the criteria, the Alt is exposed.  
   - - **Value:** Alternate
1. **Two AFs. One meets the criteria. Alt is not present, but substructure is present (Lbl with content ‘.’).**
   - **Formula:** 𝑥 = 𝑦 = 1
   - **AFs:**
   - - **1.**
   - - - **Relationship:** Supplement 
   - - - **Subtype:** application/mathml+xml
   - - **2.**
   - - - **Relationship:** Source
   - - - **Subtype:** application/x-tex
   - **Expected Result:** Under math is Lbl with additional content.
   - - **Value:** \<math\> \<mi\>&#x1d465;\</mi\> \<mo\>=\</mo\> \<mi\>&#x1d466;\</mi\> \<mo\>=\</mo\> \<mn\>1\</mn\> \</math\>
