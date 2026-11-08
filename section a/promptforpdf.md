# Prompts Log: Project 1 — PDF Studio
**Internship:** Bharat Space Education Research Centre — GenAI Internship (Week 2)  
**Project:** Project 1: PDF Studio (Tough)  
**Author / Intern:** Karan Bagul Cohort Participant  
**Environment:** Local Browser / 100% Offline / Zero API Keys  

---

## Prompt 1: Initial Architecture & Offline Foundation
```text
You are an expert web systems engineer. I am building "PDF Studio", an entirely client-side, 100% offline PDF manipulation tool running in the browser. 

Create the base scaffolding in a single, clean interface:
1. Ensure all PDF operations run locally in browser memory without sending any network request.
2. Include a top bar with an offline status indicator ("Wi-Fi Off • 100% Client-Side"), document metadata display, and an 'Open PDF' / 'Append PDF' button.
3. Include an interactive responsive thumbnail grid using HTML5 Canvas and client-side rendering (pdf.js / pdf-lib).
4. Organize the top/side toolbar into 5 distinct groups:
   - Group 1: Organise
   - Group 2: Edit
   - Group 3: Stamp
   - Group 4: Convert
   - Group 5: Finish
5. Provide a global state container holding loaded pages as an array of page objects with IDs, page indices, rotation angles, and modified overlays.
```

---

## Prompt 2: Group 1 (Organise Tools)
```text
Let's build Group 1: Organise for PDF Studio.

Implement the following 9 operations on the active document:
1. Merge: Append pages from another loaded PDF.
2. Split: Select page ranges (e.g. 1-5, 8, 11-15) and export them into a separate PDF.
3. Reorder: Drag-and-drop page cards in the grid to instantly swap order in the document manifest.
4. Rotate: Rotate selected pages or all pages clockwise in 90-degree steps.
5. Delete: Remove individual or selected pages with confirmation.
6. Duplicate: Clone any selected page and place it immediately following the source page.
7. Extract Pages: Select specific pages and download them as a standalone PDF.
8. Insert Blank Page: Insert an empty A4 page at any arbitrary position.
9. Crop: Set visual margin crop boundaries per page.

Ensure changes update the visual canvas grid immediately and persist when generating the assembled PDF via pdf-lib.
```

---

## Prompt 3: Group 2 (Edit Tools & True Redaction)
```text
Now implement Group 2: Edit for PDF Studio.

Add an interactive single-page editor modal with canvas annotation layers:
1. Add Text: Insert editable text labels with adjustable font size, font color, and coordinates.
2. Add Image: Upload a local image (PNG/JPEG) and stamp it onto the page.
3. Highlight: Semi-transparent colored box overlay.
4. Draw: Freehand pen tool with variable line thickness and palette picker.
5. Shapes: Draw rectangle, ellipse, and line vectors.
6. Whiteout: Place solid opaque white boxes over content.
7. Sign: Draw a signature on a touch/mouse pad or import a signature image, then stamp it.
8. Fill Forms: Detect and update form text fields if AcroForm fields are present.
9. Redact (CRITICAL REQUIREMENT): Implement true redaction. When a user marks an area for redaction:
   - Do NOT just place a black box over the vector layer.
   - Rasterize the target page onto a high-resolution canvas.
   - Paint the solid black rectangle directly into the raster pixel data.
   - Replace the original vector page object with this sanitized flattened page in the final PDF, guaranteeing that underlying text cannot be selected, copied, or found via Ctrl+F search in Adobe Reader or Chrome.
```

---

## Prompt 4: Group 3 (Stamp Tools)
```text
Implement Group 3: Stamp tools:
1. Watermark: Overlay diagonal or centered text (e.g., "CONFIDENTIAL", "DRAFT") across all pages with configurable opacity, font size, and rotation angle.
2. Page Numbers: Dynamically add page numbers formatted as "Page X of Y" or "X" with position options: Bottom-Center, Bottom-Right, Top-Right.
3. Header and Footer: Add custom running text in the top or bottom margins across all or specific page ranges.
4. Date Stamp: Insert current localized date and time stamp at the selected corner of each page.

Ensure all stamps embed directly into the pdf-lib coordinate system before final save.
```

---

## Prompt 5: Group 4 (Convert Tools)
```text
Implement Group 4: Convert tools:
1. Images to PDF: Allow selecting multiple JPG/PNG images and convert them sequentially into a single multi-page PDF document.
2. PDF to Images: Render each page of the active document onto offscreen canvases and export them as high-resolution PNG or JPEG downloads (individual or bundled).
3. PDF to Text: Extract raw text content from all pages using pdf.js textContent streams and download as a clean .txt file.
```

---

## Prompt 6: Group 5 (Finish Tools & Size Comparison)
```text
Implement Group 5: Finish tools:
1. Compress (CRITICAL REQUIREMENT):
   - Measure and store original file size in bytes.
   - Process the document by stripping unreferenced metadata, optimizing structural trees, and downsampling embedded images.
   - Display explicit Before vs After file size comparison in the UI (e.g., "Original: 12.8 MB | Compressed: 5.2 MB | 59% smaller").
2. Edit Details: View and edit PDF metadata properties (Title, Author, Subject, Keywords).
3. Remove Details: Strip all metadata (creator, producer, author, creation date) for privacy sanitization.
4. Compare Two PDFs: Implement a side-by-side split screen view with synchronized scrolling to visually compare two versions of a document.
```

---

## Prompt 7: Verification & "Done When" Validation
```text
Review the completed PDF Studio application against the required "Done When" criteria:
1. Wi-Fi Off test: Confirm no remote CDN calls occur once loaded; all scripts and assets function offline.
2. 50-page PDF stress test: Ensure smooth reordering, rendering, and export of a 50-page document without browser memory leaks or tab crashes.
3. External PDF reader test: Ensure exported PDFs open without errors in Adobe Acrobat Reader and Google Chrome.
4. Redaction test: Verify that searching for redacted text in an external viewer yields 0 results.
5. Compression feedback: Verify accurate before and after size readouts.
Provide bug fixes for any edge cases discovered during testing.
```