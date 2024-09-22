# Tutorial-Generate-a-PIAIC-Student-ID-Card-in-Python
### Tutorial: Generate a PIAIC Student ID Card in Python

In this tutorial, you'll learn how to create a PIAIC (Presidential Initiative for Artificial Intelligence and Computing) student ID card using Python and the Pillow (PIL) library. We'll design the card with custom images, text, and rounded corners.

#### Prerequisites
Before you begin, make sure you have the following:
1. **Python Installed**: Ensure Python is installed on your system.
2. **Pillow (PIL) Library**: Install the Pillow library for image processing:
   ```bash
   pip install pillow
   ```
3. **Images and Fonts**: You'll need:
   - A background image for the card.
   - A card holder (student) image.
   - The PIAIC logo.
   - A TrueType font (.ttf) file for custom fonts.

### Steps to Create the ID Card

#### Step 1: Set Up the Project

Create a folder for your project, and place the required images and fonts inside it.

#### Project Structure Example:
```
PIAIC_ID_Card/
├── background.jpeg
├── card_holder.jpg
├── piaic_logo.png
├── Arial.ttf
├── script.py
```

#### Step 2: Import Required Libraries
You need to import `PIL` (Pillow) libraries to handle image manipulation and font rendering.

```python
from PIL import Image, ImageDraw, ImageFont, ImageOps
from IPython.display import display
```

#### Step 3: Define User Information

You will store the student’s information as variables that will be displayed on the card.

```python
# Student Information
name = "Ameer Hamza"                        # Replace with your 
rollno = "PIAIC247168"
distance = "No"
city = "Karachi"
center = "Baharia Auditorium"
campus = "Karsaz"
day_time = "Sunday-09:00 AM to 01:00 PM"
batch = "61"
```

#### Step 4: Define Image and Font Paths

Add the paths to your images and font file.

```python
# Paths to images and font
background_image_path = 'background.jpeg'      # Background image
card_holder_image_path = 'card_holder.jpg'     # Student image
logo_image_path = 'piaic_logo.png'             # PIAIC logo
font_path = 'Arial.ttf'                        # Font file for text
```

#### Step 5: Create a Blank ID Card with Rounded Corners

We'll create a blank card and add rounded corners to it. Set the dimensions for your card (e.g., 600x400 pixels).

```python
# Create a blank ID card
card_width, card_height = 600, 400
card_color = (255, 255, 255, 255)  # White background
id_card = Image.new("RGBA", (card_width, card_height), card_color)

# Add rounded corners
radius = 30  # Radius for rounded corners
rounded_card = Image.new("L", (card_width, card_height), 0)
draw_rounded = ImageDraw.Draw(rounded_card)
draw_rounded.rounded_rectangle([(0, 0), (card_width, card_height)], radius, fill=255)
id_card = ImageOps.fit(id_card, (card_width, card_height), centering=(0.5, 0.5))
id_card.putalpha(rounded_card)
```

#### Step 6: Add the Background Image

Next, load the background image and paste it onto the card.

```python
# Draw the card background
background = Image.open(background_image_path).resize((card_width, card_height)).convert("RGBA")
id_card.paste(background, (0, 0), mask=rounded_card)
```

#### Step 7: Add the Card Holder (Student) Image

The student’s picture will be placed on the top-right corner of the ID card with a border.

```python
# Load and resize the card holder image
card_holder_image = Image.open(card_holder_image_path).convert("RGBA")
card_holder_image = card_holder_image.resize((150, 150))

# Draw the card holder image in a box with a border
border_thickness = 5
image_x_position = card_width - card_holder_image.width - 50
image_y_position = 50  # Top-right corner
border_color = (0, 0, 0, 255)  # Black border
bordered_image = Image.new("RGBA", (card_holder_image.size[0] + 2 * border_thickness, card_holder_image.size[1] + 2 * border_thickness), border_color)
bordered_image.paste(card_holder_image, (border_thickness, border_thickness))
id_card.paste(bordered_image, (image_x_position, image_y_position), bordered_image)
```

#### Step 8: Add the PIAIC Logo

The PIAIC logo will be placed in the center of the card. We can adjust its transparency to 50%.

```python
# Load and resize the PIAIC logo
logo = Image.open(logo_image_path).convert("RGBA")
logo = logo.resize((150, 150))  # Resize logo

# Adjust the logo's transparency to 50%
logo_alpha = logo.getchannel('A')
new_alpha = logo_alpha.point(lambda p: p * 0.5)  # 50% transparency
logo.putalpha(new_alpha)

# Paste the logo in the center of the card
logo_position = ((card_width - logo.width) // 2, (card_height - logo.height) // 2)
id_card.paste(logo, logo_position, logo)
```

#### Step 9: Add Text to the Card

We'll use the student information variables and display them on the left side of the card.

```python
# Load the font for text
font = ImageFont.truetype(font_path, 24)

# Draw the text on the left side
draw = ImageDraw.Draw(id_card)
padding = 30
text_x = padding
text_y = 50  # Adjust Y position as needed
line_spacing = 40  # Adjust line spacing

# Text labels to display
text_labels = [
    f"Name: {name}",
    f"Roll No: {rollno}",
    f"Distance Learning: {distance}",
    f"City: {city}",
    f"Center: {center}",
    f"Campus: {campus}",
    f"Day/Time: {day_time}",
    f"Batch: {batch}"
]

# Draw each line of text
for line in text_labels:
    draw.text((text_x, text_y), line, font=font, fill="black")
    text_y += line_spacing
```

#### Step 10: Add the "PIAIC ID Card" Label at the Top

We will add a title "PIAIC ID Card" at the top of the card, in bold.

```python
# Try loading the bold font for the title
try:
    label_font = ImageFont.truetype("Arial-Bold.ttf", 40)  # Path to bold font
except OSError:
    label_font = ImageFont.load_default()  # Fallback to default if not found

# Draw the "PIAIC ID Card" text at the top
draw = ImageDraw.Draw(id_card)
piaic_label = "PIAIC ID Card"
label_bbox = draw.textbbox((0, 0), piaic_label, font=label_font)
text_width = label_bbox[2] - label_bbox[0]
label_x = (card_width - text_width) // 2
label_y = 10  # Adjust Y position
draw.text((label_x, label_y), piaic_label, font=label_font, fill="black")
```

#### Step 11: Save and Display the ID Card

Finally, save and display the completed ID card.

```python
# Save the ID card to a file
output_path = "PIAIC_ID_card.png"
id_card.save(output_path)

# Display the ID card
id_card.show()
```

### Full Code:

Here is the complete code for generating the PIAIC Student ID card:

```python
from PIL import Image, ImageDraw, ImageFont, ImageOps
from IPython.display import display

# Student Information
name = "Ameer Hamza"
rollno = "PIAIC247168"
distance = "No"
city = "Karachi"
center = "Baharia Auditorium"
campus = "Karsaz"
day_time = "Sunday-09:00 AM to 01:00 PM"
batch = "61"

# Paths to images and font
background_image_path = 'background.jpeg'
card_holder_image_path = 'card_holder.jpg'
logo_image_path = 'piaic_logo.png'
font_path = 'Arial.ttf'

# Create a blank ID card
card_width, card_height = 600, 400
card_color = (255, 255, 255, 255)
id_card = Image.new("RGBA", (card_width, card_height), card_color)

# Add rounded corners
radius = 30
rounded_card = Image.new("L", (card_width, card_height), 0)
draw_rounded = ImageDraw.Draw(rounded_card)
draw_rounded.rounded_rectangle([(0, 0), (card_width, card_height)], radius, fill=255)
id_card = ImageOps.fit(id_card, (card_width, card_height), centering=(0.5, 0.5))
id_card.putalpha(rounded_card)

# Add background image
background = Image.open(background_image_path).resize((card_width, card_height)).convert("RGBA")
id_card.paste(background, (0, 0), mask=

rounded_card)

# Add card holder image
card_holder_image = Image.open(card_holder_image_path).convert("RGBA")
card_holder_image = card_holder_image.resize((150, 150))
border_thickness = 5
image_x_position = card_width - card_holder_image.width - 50
image_y_position = 50
border_color = (0, 0, 0, 255)
bordered_image = Image.new("RGBA", (card_holder_image.size[0] + 2 * border_thickness, card_holder_image.size[1] + 2 * border_thickness), border_color)
bordered_image.paste(card_holder_image, (border_thickness, border_thickness))
id_card.paste(bordered_image, (image_x_position, image_y_position), bordered_image)

# Add logo
logo = Image.open(logo_image_path).convert("RGBA")
logo = logo.resize((150, 150))
logo_alpha = logo.getchannel('A')
new_alpha = logo_alpha.point(lambda p: p * 0.5)
logo.putalpha(new_alpha)
logo_position = ((card_width - logo.width) // 2, (card_height - logo.height) // 2)
id_card.paste(logo, logo_position, logo)

# Load fonts
try:
    label_font = ImageFont.truetype("Arial-Bold.ttf", 40)
except OSError:
    label_font = ImageFont.load_default()
    
try:
    font = ImageFont.truetype(font_path, 24)
except OSError:
    font = ImageFont.load_default()

# Add title
draw = ImageDraw.Draw(id_card)
piaic_label = "PIAIC ID Card"
label_bbox = draw.textbbox((0, 0), piaic_label, font=label_font)
text_width = label_bbox[2] - label_bbox[0]
label_x = (card_width - text_width) // 2
label_y = 10
draw.text((label_x, label_y), piaic_label, font=label_font, fill="black")

# Add student info
padding = 30
text_x = padding
text_y = 100
line_spacing = 40
text_labels = [
    f"Name: {name}",
    f"Roll No: {rollno}",
    f"Distance Learning: {distance}",
    f"City: {city}",
    f"Center: {center}",
    f"Campus: {campus}",
    f"Day/Time: {day_time}",
    f"Batch: {batch}"
]

for line in text_labels:
    draw.text((text_x, text_y), line, font=font, fill="black")
    text_y += line_spacing

# Save and display
output_path = "PIAIC_ID_card.png"
id_card.save(output_path)
id_card.show()
```

### Conclusion:

By following this tutorial, you have successfully created a PIAIC Student ID card with custom text, images, and styles. You can further modify the card design as per your needs, such as adding additional information, changing fonts, or experimenting with different layouts.
