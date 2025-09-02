# Image Stego Extraction 

This repo documents a digital forensics project where I analyzed a suspicious image and pulled out hidden data using steganography tools. The work was done inside an Ubuntu VM with evidence preserved step-by-step.

## Goal
Take an ordinary-looking cat photo, run forensic analysis to see if anything was hidden inside, and extract whatever is there and document the results.

## Stack
- **Ubuntu VM**
- **Binwalk**: scan and extract embedded files
- **Hexedit / HxD**: examine file headers in hex
- **xdg-open**: quick validation of carved files
- **Screenshots**: documented every major step

## Findings
- Original cat image contained two hidden JPEG headers buried at offsets.
- One hidden file was corrupted but still showed valid JPEG markers in hex.
- One hidden file opened successfully as a separate image (a cat sticking its tongue out).
- Both the process and results are documented in report.md
