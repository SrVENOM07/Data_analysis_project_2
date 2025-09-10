# Data_analysis_project_2
# 📄 PDF to Kannada Spreadsheet Converter

A simple React-based web tool that helps convert **voter list OCR data** into a clean, structured spreadsheet-ready format with Kannada headers.  

This tool is useful when working with government voter lists, OCR-extracted data, or similar datasets where the input may have different separators (`|`, tab, comma, or space).  

---

## 🚀 Features
- 📝 **Manual Data Input** – Paste OCR or extracted text directly.  
- ⚡ **Auto Processing** – Detects separators (`|`, tab, comma, space) and splits into columns.  
- 🎯 **Kannada Headers** – Automatically structures data into required voter-list fields:  
  - ಕ್ರಮ ಸಂಖ್ಯೆ  
  - ಮನೆ ಸಂಖ್ಯೆ  
  - ಮತದಾರರ ಹೆಸರು  
  - ಸಂಬಂಧ  
  - ಸಂಬಂಧಿಯ ಹೆಸರು  
  - ಲಿಂಗ  
  - ವಯಸ್ಸು  
  - ಮತದಾರರ ಗುರುತಿನ ಚೀಟಿ ಸಂಖ್ಯೆ  
- 👀 **Data Preview** – View first 10 rows of processed data.  
- 📋 **One-click Copy** – Copy clean, tab-separated data ready for pasting into **Excel / Google Sheets**.  

---

## 🛠️ Tech Stack
- **React 18 (via CDN)**
- **Tailwind CSS** for styling
- **Babel** (in-browser JSX transform)
- Unicode emoji icons (lightweight Lucide replacement)

---

## 📂 Project Structure
