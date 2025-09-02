# Image Stego Extraction
TL;DR: I took a normal looking cat photo, found extra JPEGs hiding inside it, carved them out, and saved the hidden image as evidence. Everything below is the exact workflow with proofs.

## 1. Setup (Ubuntu VM)
Made a working directory in my repo: ~/projects/image-stego-extraction/work
Installed the tools I needed:
- sudo apt update
- sudo apt install -y binwalk exiftool hexedit

## 2. Baseline file
Dropped the provided cat image into work/ and verified it’s there.
- ls -lh work
- (Original is committed as work/cat_photo.jpg.)

## 3. San for embedded files
Ran binwalk on the image to see what’s inside.
- cd work
- binwalk cat_photo.jpg

### Key result
- Multiple JPEG headers detected:
- 0 (main image)
- 416788 (0x65C14)
- 998740 (0xF3D54)

## 4. Carve-extract embedded files
Told binwalk to carve anything it finds.
- binwalk --dd='.*' cat_photo.jpg
- ls -lh _cat_photo.jpg.extracted

This created _cat_photo.jpg.extracted/ with two files named after their offsets: 65C14 and F3D54.

## 5. Verify: open the carved objects
Tried to open both.
- xdg-open _cat_photo.jpg.extracted/65C14
- xdg-open _cat_photo.jpg.extracted/F3D54

- 65C14 failed to open (viewer error: invalid JPEG structure, “two SOI markers”). Classic partially messy embed.
- F3D54 opened cleanly as a separate cat image.

## 6. Preserve the good hit
Copied the clean hidden image out with a proper name and kept the original extracted folder intact.
- cp _cat_photo.jpg.extracted/F3D54 hidden_cat.jpg

Committed as work/hidden_cat.jpg.

## Findings
- The “normal” JPEG had two additional JPEGs embedded at offsets 0x65C14 and 0xF3D54.
- The object at 0xF3D54 is a valid, viewable image (now preserved as hidden_cat.jpg).
- The object at 0x65C14 contains JPEG markers but is malformed for straight viewing. It can be repaired with manual hex carving if needed.
