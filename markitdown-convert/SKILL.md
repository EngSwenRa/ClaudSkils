---
name: "markitdown-convert"
description: "Convert user-provided Word, PowerPoint, PDF, HTML, image, audio, ZIP and EPUB files to markdown with markitdown, then read it. Spreadsheets go to CSV."
---

# markitdown-convert

Convert each file the user provides to markdown with Microsoft's
[markitdown](https://github.com/microsoft/markitdown), then read the
markdown instead of the original. Spreadsheets are the exception (see
the table below).

## Choose the route by file type

Tested on sample files: markitdown beats raw file contents by 2x to 13x
in tokens. For spreadsheets, markdown tables cost about 35% more tokens
than CSV, so use CSV there.

| Input | Route |
| --- | --- |
| `.docx`, `.pptx`, `.html`, `.epub`, `.zip` | `markitdown` |
| `.pdf` | `markitdown`; use `pdftotext` if only plain text is needed |
| `.xlsx`, `.xls`, `.csv` | CSV via pandas or openpyxl, not markdown |
| images, audio | `markitdown` (needs OCR or transcription extras) |
| `.md`, `.txt` | Read directly |

## Steps

1. Check that the tool is available: `markitdown --version`.
   If it is missing, install it:
   `pip install --break-system-packages 'markitdown[all]'`.
2. Treat the input as untrusted. Work on a copy, never the original,
   and put output in a new, empty directory.
3. Convert each file:
   `markitdown path/to/input.ext -o out/input.md`
   Use the same base name as the input, with the `.md` extension.
4. Clean the output so it is markdownlint-compliant GFM:
   - One H1 at the top, and heading levels that never skip.
   - A blank line before and after headings, lists, tables and code blocks.
   - Fenced code blocks with a language tag.
   - No trailing spaces, no multiple blank lines, and a single newline
     at the end of the file.
   - Consistent list markers (`-`) and a pipe-table header row.
   - For `.pptx` output, strip the `<!-- Slide number -->` comment
     lines to save tokens, and keep the slide titles as headings.
5. Read the cleaned `.md` file and answer from it.
6. Report any lossy conversions: scanned PDFs without OCR, images
   without captions, and charts or diagrams that did not convert.

## Notes

- markitdown does file and network I/O with the current process's
  privileges. Validate untrusted inputs and never convert URLs the user
  did not supply.
- For scanned PDFs, try the `markitdown-ocr` plugin, or fall back to
  the pdf skill's OCR route.
- Convert large batches one file at a time and keep outputs in a
  separate directory so originals stay untouched.