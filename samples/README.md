## **mathml-AF-complex test.pdf**

### Each Formula is Supposed to Contain a Specific Problem
*All techniques are fully compatible with PDF/UA-2, Well-Tagged PDF, and ready for derivation to HTML.*

<!-- This is a test comment -->

1. **Exposed AF example, two AFs: One with `Source`, one with `Supplement`. Media type is mixed-case `APPLICATION/mathml+xml`. No Alt present.**
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

2. **Not exposed AF - content exposed, two AFs: `Source` with `application/mathml+xml`, `Supplement` with `application/x-tex`. No Alt present.**
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

3. **Not exposed AF - content exposed, two AFs: Both have incorrect media types. No Alt present.**
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

4. **Not exposed AF - content exposed, two AFs: `Source` and `Alternative`. `Alternative` has the correct media type. No Alt present.**
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

5. **Exposed AF despite Alt present, two AFs: Both meet criteria. Alt is present.**
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

6. **Not exposed AF, Alt taken instead, two AFs: None meet criteria. Alt is present.**
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

7. **Pass example, Exposed AS + subctructure to be processed, two AFs: One meets criteria. No Alt, but substructure present (e.g., `Lbl` with content ‘.’).**
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
| 1 | Exposed AF, two AFs: Source + Supplement (media type in mixed-case). No Alt. | 𝑎𝑥² + 𝑏𝑥 + 𝑐 = 0 | Supplement: `APPLICATION/mathml+xml`<br>Source: `application/x-tex` | – | First AF is used. Media type is case-insensitive. | `<math> <mi>...</mi> ... </math>` |
| 2 | Not exposed AF - fallback to content. Two AFs: Source (mathml), Supplement (tex). No Alt. | 𝑥 = −𝑏 ± √(𝑏²−4𝑎𝑐)/2𝑎 | Source: `application/mathml+xml`<br>Supplement: `application/x-tex` | – | No AF qualifies. Formula content is used. | 𝑥 = −𝑏 ± √(𝑏²−4𝑎𝑐)/2𝑎 |
| 3 | Not exposed AF - fallback to content. AFs have wrong media type. No Alt. | \|−1\| = 1 | Supplement: `WRONG_MEDIA`<br>Source: `application/x-tex` | – | No AF qualifies. Formula content is used. | \|−1\| = 1 |
| 4 | Not exposed AF - fallback to content. Alternative present but ignored. No Alt. | (1234)(1101) = (1337) | Alternative: `application/mathml+xml`<br>Source: `application/x-tex` | – | Alternative ignored. Formula content used. | (1234)(1101) = (1337) |
| 5 | Exposed AF despite Alt. Two valid AFs. | sin²𝜃 + cos²𝜃 = 1 | Supplement (x2): `application/mathml+xml` | Alternate text | Alt ignored, first AF content used. | `<math display="block"> <msup> ... </math>` |
| 6 | Not exposed AF. Alt used. Two AFs: one invalid, one incomplete. | 2𝑥 + 𝑦 = 3; 𝑥 − 𝑦 = 0 | AF1 missing relationship<br>AF2: Source `application/x-tex` | Alternate | No AF qualifies. Alt text used. | Alternate |
| 7 | Exposed AF + substructure processed. Valid Supplement present. | 𝑥 = 𝑦 = 1 | Supplement: `application/mathml+xml`<br>Source: `application/x-tex` | – | First AF used. Substructure (`Lbl`) processed. | `<math> <mi>...</mi> = ... </math>` |
