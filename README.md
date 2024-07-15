# PDF Details Extraction

This project is a Python-based utility designed to extract text content and metadata from PDF documents. 
It also supports translating the extracted text to a specified language. The extracted details are saved in a structured JSON format, enhancing document management efficiency.

## Features

- Extract text content from PDF files
- Retrieve font details and text coordinates
- Translate extracted text to a specified language
- Save extracted and translated details in a JSON file
- Convert RGB color values to hexadecimal format

## Prerequisites

- Python 3.6+
- `pip` (Python package installer)

Usage
Place your PDF file in a desired location, for example, D:/inv.pdf.

Update the main function in pdf_extraction.py with your PDF file path and target language code:

python
Copy code
pdf_file = "D:/inv.pdf"  # Replace with your PDF file path
target_language = "hi"  # Replace with your language code
Run the script:

sh
Copy code
python pdf_extraction.py
The extracted and translated text details will be saved in a JSON file at the specified output path, for example, D:/translated_text.json.

{
    "0": {
        "height": 841.92,
        "width": 595.32,
        "data": [
            {
                "original_text": "Sample text",
                "translated_text": "अनुवादित पाठ",
                "font_size": 12,
                "font_family": "Times-Roman",
                "font_color": "(0, 0, 0)",
                "font_color_hex": "#000000",
                "font_style": "bold italic",
                "left": 100.0,
                "top": 200.0,
                "end_left": 150.0,
                "end_top": 220.0
            }
        ]
    }
}


Acknowledgements
PyMuPDF for PDF parsing
Googletrans for text translation
