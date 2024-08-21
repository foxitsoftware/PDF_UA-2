# Testint Tools

Installers for the latest PoCs of our tools are not hosted in this repository.
Links lead to external OneDrive folder

## Reader
**[UA-2 PoC Foxit Reader.exe](https://foxitpdf-my.sharepoint.com/:u:/g/personal/roman_toda_foxitsoftware_com/EfcLt62w7wNKkRzeVznw3ukBclPGk36SliUi0X8Qf59T8g?e=DIehPs)** 

*21.08.2024*
code name: GA_PDFReader_AllLang_Online_Bundle_v2024.3.0.26537.exe
- Support for PDF 2.0 namespaces
- Support for *RoleMapNS*
- PDDom update
- Expose AF on Formula structure element where  `NS == 2.0`, `AFRelationship == Supplement` and the `EF` entry `Subtype == application#2Fmathml+xml`

## Custom Foxit Editor (Experimental features)

**[UA-2 PoC Foxit Editor.msi](https://foxitpdf-my.sharepoint.com/:u:/g/personal/roman_toda_foxitsoftware_com/EcVqgZ39oaBOoZJAENKTPMwBAEDOwUquOqX1QH3eO471Mg?e=52sIKZ)** 

*8.12.2023*
code name: GA_PDFEditor_En_Online_Full_Continuous_v2024.1.0.23037_Continuous.msi

- Add Structured Destination
- Set *Ref* entry
- Add PDF 2.0 Namespace
- Add Associated file
- ~~Recognize math from selection~~

## NVDA

The NVDA build that is compatible with our PDDom implementation is provided and supported by https://github.com/NSoiffer 
and can be downloaded here: git@github.com:NSoiffer/nvda.git

Direct link to installer:
[updated NVDA](https://foxitpdf-my.sharepoint.com/:u:/g/personal/roman_toda_foxitsoftware_com/EX3Qnbzbt-xPrglRcZu0YGsBzCEZ3rc87zNaBo2e-LZ_DA?e=yBGRj5)

# Implementation Testing Guide

## Prerequisites:
1. **Foxit Reader:** Install this PDF reader for viewing and testing.
2. **NVDA (NonVisual Desktop Access):** Install the NVDA screen reader (experimental version).
3. **MathCat Plugin for NVDA:** (Optional) Install this plugin if you want to test math reading with NVDA.

## Testing Steps:

### 1. Set Up NVDA and Foxit Reader
- Open **NVDA** (ensure the screen reader is running).
- Open **Foxit Reader**.

### 2. Open and Test the PDF
- Open the target PDF file in **Foxit Reader**.

### 3. Simulate PDDom Inquiries
- Run the Python script `PDDomView.py` to simulate the PDDom inquiries:
  ```bash
  python PDDomView.py