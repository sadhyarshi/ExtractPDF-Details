import json
import fitz  # PyMuPDF
from googletrans import Translator

def extract_text_with_font_and_coordinates(page):
    text_with_font_and_coordinates = []

    blocks = page.get_text("dict")["blocks"]

    for b in blocks:
        for l in b.get("lines", []):
            for s in l.get("spans", []):
                font_flags = s.get("flags", 0)
                font_style = ""
                if font_flags & 1 << 0:
                    font_style += "bold "
                if font_flags & 1 << 1:
                    font_style += "italic "
                if font_flags & 1 << 2:
                    font_style += "underline "
                if font_flags & 1 << 3:
                    font_style += "strikethrough "
                if font_flags & 1 << 4:
                    font_style += "superscript "
                if font_flags & 1 << 5:
                    font_style += "subscript "
                
                font_family = s.get("font", "Unknown")
                if "bold" in font_family.lower() or "black" in font_family.lower() or "heavy" in font_family.lower():
                    font_style += "bold "
                if "italic" in font_family.lower():
                    font_style += "italic "

                text_with_font_and_coordinates.append({
                    "text": s.get("text", ""),
                    "font_size": s.get("size", 0),
                    "font_family": font_family,
                    "font_color": s.get("color", (0, 0, 0)),
                    "font_style": font_style.strip(),  # Remove trailing space
                    "left": s.get("bbox", (0, 0, 0, 0))[0],
                    "top": s.get("bbox", (0, 0, 0, 0))[1],
                    "width": s.get("bbox", (0, 0, 0, 0))[2] - s.get("bbox", (0, 0, 0, 0))[0],
                    "height": s.get("bbox", (0, 0, 0, 0))[3] - s.get("bbox", (0, 0, 0, 0))[1]
                })

    return text_with_font_and_coordinates

def translate_text(text, target_language):
    translator = Translator()
    translated_text = translator.translate(text, dest=target_language).text
    return translated_text
def rgb_to_hex(rgb):
    """
    Convert RGB color values to hexadecimal color code.
    
    Args:
        rgb (tuple): RGB color values (0-255).
    
    Returns:
        str: Hexadecimal color code.
    """
    r, g, b = rgb
    return "#{:02x}{:02x}{:02x}".format(r, g, b)

def convert_color_to_hex(rgb_color):
    """
    Convert font color from RGB to hexadecimal format and update JSON data.
    
    Args:
        item (dict): Dictionary containing font color in RGB format.
    
    Returns:
        str: Hexadecimal color code.
    """ 
    if isinstance(rgb_color, int):
        # If rgb_color is an integer, convert it to a tuple
        r = (rgb_color >> 16) & 0xFF
        g = (rgb_color >> 8) & 0xFF
        b = rgb_color & 0xFF
        rgb_color = (r, g, b)
    font_color_hex = rgb_to_hex(rgb_color)
    return font_color_hex

def main():
    pdf_file = "D:/inv.pdf"  # Replace with your PDF file path
    target_language = "hi"  # Replace with your language code

  
    translated_data = {}

    
    doc = fitz.open(pdf_file)

    for page_number in range(len(doc)):
        page = doc[page_number]
        # Get page dimensions
        page_width = page.rect.width
        page_height = page.rect.height

       
        page_data = {
            "height": page_height,
            "width": page_width,
            "data": []
        }
        
        # Append page data to translated_data
        translated_data[str(page_number)] = page_data

        print(f"Extracting text from page {page_number + 1}...")
        
        # Extract text with font and coordinates
        text_with_font_and_coordinates = extract_text_with_font_and_coordinates(page)
        print(f"Extracted {len(text_with_font_and_coordinates)} text spans.")

        # Translate and append text data to the respective page data
        for item in text_with_font_and_coordinates:
            original_text = item["text"]
            translated_text = translate_text(original_text, target_language)
            end_left = item["left"] + item["width"]  # Calculate end left coordinate
            end_top = item["top"] + item["height"]  # Calculate end top coordinate
            font_color_hex = convert_color_to_hex(item["font_color"])  # Convert font color to hex
            page_data["data"].append({
                "original_text": original_text,
                "translated_text": translated_text,
                "font_size": item["font_size"],
                "font_family": item["font_family"],
                "font_color": item["font_color"],  # Use hex color code
                "font_color_hex":font_color_hex, #
                "font_style": item["font_style"],  # Include font style
                "left": item["left"],
                "top": item["top"],
                "end_left": end_left,
                "end_top": end_top
            })

    # Output JSON file path
    output_path = "D:/translated_text.json"

    # Write translated_data to JSON file
    with open(output_path, "w", encoding="utf-8") as json_file:
        json.dump(translated_data, json_file, ensure_ascii=False, indent=4)
    print(f"JSON file saved at: {output_path}")

if __name__ == "__main__":
    main()
