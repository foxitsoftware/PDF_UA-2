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

1. **AF with Supplement and Media Type is in Mixed Upper and Lowercase (`*APPLICATION/mathml+xml)`):**
   - **Expected Result:** Should return the content of the first AF in the array. The media type is case insensitive.
   - **Value:** \<math\> \<mi\>&#x1d44e;\</mi\> \<mo\>&#x2062;\</mo\> \<msup\> \<mi\>&#x1d465;\</mi\> \<mn\>2\</mn\> \</msup\> \<mo\>+\</mo\> \<mi\>&#x1d44f;\</mi\> \<mo\>&#x2062;\</mo\> \<mi\>&#x1d465;\</mi\> \<mo\>+\</mo\> \<mi\>&#x1d450;\</mi\> \<mo\>=\</mo\> \<mn\>0\</mn\> \</math\>
2. **Two AFs. One with Source, One with Supplement. The Source with media type `application/mathml+xml` and Supplement with `application/x-tex`. No Alt present.**
   - **Expected Result:**  No AF meets criteria. No Alt present. It should return the content items under Formula structure element.
   - **Value:** 𝑥=−𝑏±√𝑏2−4𝑎𝑐2𝑎
3. **Two AFs. One with Source, One with Supplement. Both don't have the correct media type, no Alt present.**
   - **Expected Result:** No AF meets criteria. No Alt present. It should return the content items under Formula structure element.
   - **Value:** |−1|=1
4. **Two AFs. One with Source, One with Alternative. Alternative with the correct media type.**
   - **Expected Result:**  Alternative is not defined yet.
   - **Value:** (1234)(1101)=(1337)
5. **Two AFs Both Meet Criteria, Alt Present.**
   - **Expected Result:** Just the content of the first AF should be exposed, Alt is ignored.
   - **Value:** \<math display="block"\> \<msup\> \<mi mathvariant="normal"\>sin\</mi\> \<mn\>2\</mn\> \</msup\> \<mo rspace="0.167em"\>&#x2061;\</mo\> \<mi\>&#x1d703;\</mi\> \<mo\>+\</mo\> \<msup\> \<mi mathvariant="normal"\>cos\</mi\> \<mn\>2\</mn\> \</msup\> \<mo rspace="0.167em"\>&#x2061;\</mo\> \<mi\>&#x1d703;\</mi\> \<mo\>=\</mo\> \<mn\>1\</mn\> \</math\>
6. **Two AFs None Meet Criteria, Alt Present.**
   - **Expected Result:** If no associated file meets the criteria, the Alt is exposed.  
   - **Value:** Alternate
7. **Two AFs One Meets the Criteria, Alt is Not Present, but Substructure is Present (Lbl with content ‘.’).**
   - **Expected Result:** Under math is Lbl with additional content.
   - **Value:** \<math\> \<mi\>&#x1d465;\</mi\> \<mo\>=\</mo\> \<mi\>&#x1d466;\</mi\> \<mo\>=\</mo\> \<mn\>1\</mn\> \</math\>
