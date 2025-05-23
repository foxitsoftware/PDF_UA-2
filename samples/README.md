## **mathml-AF-complex test.pdf**

### Each Formula is Supposed to Contain a Specific Problem
*All techniques are fully compatible with PDF/UA-2, Well-Tagged PDF, and ready for derivation to HTML.*

<!-- This is a test comment -->
**Exposed AF example**
1. **Two AFs: One with `Source`, one with `Supplement`. Media type is mixed-case `APPLICATION/mathml+xml`. No Alt present.**
   - **Formula:** 𝑎𝑥² + 𝑏𝑥 + 𝑐 = 0
   - **AFs:**
     - **1.**
       - **Relationship:** Supplement  
       - **Subtype:** APPLICATION/mathml+xml  
     - **2.**
       - **Relationship:** Source  
       - **Subtype:** application/x-tex
   - **Expected Result:** Should return the content of the first AF in the array. Media type is case-insensitive.
     - **Value:**
     ```xml
     <math> <mi>&#x1d44e;</mi> <mo>&#x2062;</mo> <msup> <mi>&#x1d465;</mi> <mn>2</mn> </msup> <mo>+</mo> <mi>&#x1d44f;</mi> <mo>&#x2062;</mo> <mi>&#x1d465;</mi> <mo>+</mo> <mi>&#x1d450;</mi> <mo>=</mo> <mn>0</mn> </math>
     ```

**Not exposed AF - content exposed**
2. **Two AFs: `Source` with `application/mathml+xml`, `Supplement` with `application/x-tex`. No Alt present.**
   - **Formula:** 𝑥 = −𝑏 ± √(𝑏²−4𝑎𝑐) / 2𝑎
   - **AFs:**
     - **1.**
       - **Relationship:** Source  
       - **Subtype:** application/mathml+xml  
     - **2.**
       - **Relationship:** Supplement  
       - **Subtype:** application/x-tex
   - **Expected Result:** No AF meets criteria. No Alt present. Content under Formula structure should be returned.
   - **Value:** 𝑥 = −𝑏 ± √(𝑏²−4𝑎𝑐) / 2𝑎

**Not exposed AF - content exposed**
3. **Two AFs: Both have incorrect media types. No Alt present.**
   - **Formula:** |−1| = 1
   - **AFs:**
     - **1.**
       - **Relationship:** Supplement  
       - **Subtype:** WRONG_MEDIA  
     - **2.**
       - **Relationship:** Source  
       - **Subtype:** application/x-tex
   - **Expected Result:** No AF meets criteria. No Alt present. Content under Formula structure should be returned.
   - **Value:** |−1| = 1

**Not exposed AF - content exposed**
4. **Two AFs: `Source` and `Alternative`. `Alternative` has the correct media type. No Alt present.**
   - **Formula:** (1234)(1101) = (1337)
   - **AFs:**
     - **1.**
       - **Relationship:** Alternative  
       - **Subtype:** application/mathml+xml  
     - **2.**
       - **Relationship:** Source  
       - **Subtype:** application/x-tex
   - **Expected Result:** Alternative relationship is not currently processed. Should be ignored.
   - **Value:** (1234)(1101) = (1337)

**Exposed AF despite Alt present**
5. **Two AFs: Both meet criteria. Alt is present.**
   - **Formula:** sin²𝜃 + cos²𝜃 = 1
   - **AFs:**
     - **1.**
       - **Relationship:** Supplement  
       - **Subtype:** application/mathml+xml  
     - **2.**
       - **Relationship:** Supplement  
       - **Subtype:** application/mathml+xml
   - **Alt:** Alternate text
   - **Expected Result:** Return content of first AF. Alt is ignored.
   - **Value:**
     ```xml
     <math display="block"> <msup> <mi mathvariant="normal">sin</mi> <mn>2</mn> </msup> <mo rspace="0.167em">&#x2061;</mo> <mi>&#x1d703;</mi> <mo>+</mo> <msup> <mi mathvariant="normal">cos</mi> <mn>2</mn> </msup> <mo rspace="0.167em">&#x2061;</mo> <mi>&#x1d703;</mi> <mo>=</mo> <mn>1</mn> </math>
     ```

**Not exposed AF, Alt taken instead**
6. **Two AFs: None meet criteria. Alt is present.**
   - **Formula:** 2𝑥 + 𝑦 = 3; 𝑥 − 𝑦 = 0
   - **AFs:**
     - **1.**
       - **Relationship:** *(missing — should be specified)*  
       - **Subtype:** application/mathml+xml  
     - **2.**
       - **Relationship:** Source  
       - **Subtype:** application/x-tex
   - **Alt:** Alternate
   - **Expected Result:** No AF meets criteria. Alt text should be exposed.
   - **Value:** Alternate

**Exposed AS + substructure to be processed**
7. **Two AFs: One meets criteria. No Alt, but substructure present (e.g., `Lbl` with content ‘.’).**
   - **Formula:** 𝑥 = 𝑦 = 1
   - **AFs:**
     - **1.**
       - **Relationship:** Supplement  
       - **Subtype:** application/mathml+xml  
     - **2.**
       - **Relationship:** Source  
       - **Subtype:** application/x-tex
   - **Expected Result:** Return content of first AF. Substructure like `Lbl` should also be processed.
   - **Value:**
     ```xml
     <math> <mi>&#x1d465;</mi> <mo>=</mo> <mi>&#x1d466;</mi> <mo>=</mo> <mn>1</mn> </math>
     ```

---

## **Variance - Wikipedia - Tagged - AF - Ref(OBJR) - NS.pdf**

- **PDF Header:** 2.0  
- **Namespace on Structure Elements:** 2.0  
- **AF on Formula:** With `AFRelationship == Supplement`  
- **First Formula:** Contains `Lbl` (Reference to `FENote`)  
- **FENote:** Contains reference


## **mathml-AF-complex test.pdf**

| # | Description | Formula | AFs | Alt | Expected Result | Value |
|---|-------------|---------|-----|-----|-----------------|-------|
| 1 | Exposed AF. Two AFs: `Source` + `Supplement`. Mixed-case media type. No Alt. | 𝑎𝑥² + 𝑏𝑥 + 𝑐 = 0 | Supplement: `APPLICATION/mathml+xml`<br>Source: `application/x-tex` | – | Use first AF. Media type is case-insensitive. | `<math> <mi>...</mi> ... </math>` |
| 2 | Not exposed AF. Fallback to content. Two AFs: Source (mathml), Supplement (tex). | 𝑥 = −𝑏 ± √(𝑏²−4𝑎𝑐) / 2𝑎 | Source: `application/mathml+xml`<br>Supplement: `application/x-tex` | – | No AF qualifies. Show formula content. | 𝑥 = −𝑏 ± √(𝑏²−4𝑎𝑐) / 2𝑎 |
| 3 | Not exposed AF. Both AFs invalid. Fallback to content. | \|−1\| = 1 | Supplement: `WRONG_MEDIA`<br>Source: `application/x-tex` | – | No AF qualifies. Show formula content. | \|−1\| = 1 |
| 4 | Not exposed AF. Alternative is ignored. | (1234)(1101) = (1337) | Alternative: `application/mathml+xml`<br>Source: `application/x-tex` | – | Alt ignored. Show formula content. | (1234)(1101) = (1337) |
| 5 | Exposed AF despite Alt. Both AFs valid. | sin²𝜃 + cos²𝜃 = 1 | Supplement (x2): `application/mathml+xml` | Alternate text | Alt ignored. Show first AF content. | `<math display="block"> <msup> ... </math>` |
| 6 | Not exposed AF. Alt is used. One AF invalid. | 2𝑥 + 𝑦 = 3; 𝑥 − 𝑦 = 0 | AF1 missing relationship<br>AF2: Source `application/x-tex` | Alternate | No AF qualifies. Show Alt. | Alternate |
| 7 | Exposed AF + substructure processed. One valid AF. | 𝑥 = 𝑦 = 1 | Supplement: `application/mathml+xml`<br>Source: `application/x-tex` | – | Use first AF. Process `Lbl` substructure. | `<math> <mi>...</mi> = ... </math>` |
