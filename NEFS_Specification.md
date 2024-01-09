# NEFS Specification v.1.0.0

## 1. Definition
NEFS (NFC Exchange File System) is a rudimentary file system for NFC devices. It is based on the NDEF (NFC Data Exchange Format) standard and uses the NDEF message format as a foundation for its entry based file system structure. This allows the NEFS file system to be compatible with every NFC device that supports NDEF storage. Each file system entry is stored as a separate NDEF text record of the “Well Known Type (TNF 1)”. While this is not the most space efficient way of storing data, it allows for a high level of compatibility with different NFC readers, NDEF parsers and applications. *(See “5 Potential improvements” for
more information)*

## 2. Entry types

### File System Header
 - The file system header signals the beginning of the file system.
 - It consists of the 4 byte signature string “NEFS”, immediately followed by the media
name that can be freely defined by the user.

### Directory Header
 - The directory header signals the beginning of a directory.
 - It consists of the 1 byte flag “d”, immediately followed by the directory name.

### Directory Tender
 - The directory tender signals the end of a directory.
 - It consist of the 1 byte flag “e”, immediately followed by the directory name.

### Filename Entry
 - The filename entry signals the beginning of a file.
 - It consists of the 1 byte flag “f”, immediately followed by the file’s filename.
 - Every filename entry is immediately followed by it’s corresponding data entry.

### Data Entry
 - The data entry consists of a file’s data, either stored as a data URI scheme in a
NDEF “Well Known Type (TNF 1)” text record or, in the case of text files, as a text
record directly.

### File System Tender
 - The file system tender signals the end of the file system.
 - It consists of the 4 byte signature string “NEFS”, immediately followed by the 3 byte
string “end”.

## 3. Rule set
1. The file system always starts with a file system header entry.
2. Every entry following the file system header entry is considered “contained inside the file system”.
3. The file system header entry is considered “the start of the root directory”.
4. Every entry following a directory header entry is interpreted as contained inside the directory.
5. A directory MUST contain at least one filename entry and it’s corresponding data entry.
6. Every directory is closed by a directory tender entry.
7. A filename entry is NOT allowed to contain the character “/”.
8. Two or more identical filename entries are NOT allowed to exist inside of the same directory.
9. Every filename entry is always immediately followed by a data entry.
10. The file system always ends with a file system tender entry.
11. Every entry following the file system tender entry is NOT considered as “contained inside the file system”.

## 4. File System Structure – an example
In this example, each bullet point represents a separate NDEF record:

- NEFSamalsNFCstorage
- fText.txt
- This text file is contained inside the root directory!
- dMedia
- fImage.jpg
- data:image/jpeg;base64,/9j/
2wBDABALDA4MChAODQ4SERATGCgaGBYWGDEjJR0oOjM9PDkzODdASFxOQERXRTc
4UG1RV19iZ2hnPk1xeXBkeFxlZ2P/
2wBDARESEhgVGC8aGi9jQjhCY2NjY2NjY2NjY2NjY2NjY2NjY2NjY2NjY2NjY2NjY2NjY2
NjY2NjY2NjY2NjY2NjY2P/wAARCAAZABkDASIAAhEBAxEB/
8QAGQAAAwADAAAAAAAAAAAAAAAAAAQFAgMG/
8QALRAAAgEDAwIEBAcAAAAAAAAAAQIDAAQRBRIxEyEUMkFRIlNUkmFxgZGhsdH/
xAAVAQEBAAAAAAAAAAAAAAAAAAAAAv/
EABQRAQAAAAAAAAAAAAAAAAAAAAD/2gAMAwEAAhEDEQA/
AOxa9tl626dB0GCy9/ITjAP55FLtrWmq0qm7QGLO/
s2FwcHvj3qW2jXQu0uRsVp7ppbpC3mVX3R4/EYA/
WsLXSr06fFaTeIjLSRmYtcKyjDb2KAcHP8AdFK51vTREJGu0VCSMkEcc+nHfninOvD86
P7xUW9sbx7hfClndUCLctMDlScssikfEPbAq14a1+nj+wUC97YR3hy7FSEKrj0JOc/
xWp9KWV2eSY7id2VQDvz39+f2qjRQJWenLayrJ1C7hNnGBjt/lO0UUH//2Q==
- dDocuments
- fHello world.txt
- Hello world! This text file is contained in “/Media/Documents/”.
- eDocuments
- eMedia
- NEFSend

## 5. Potential improvements
As previously stated in the “1 Definition” section of this document, storing data as a data URI scheme in a NDEF “Well Known Type (TNF 1)” text record is not the most space efficient way, but it guarantees a high level of compatibility. If a more space efficient way is required, one could store the raw data as a “MIME type record (TNF 2)”, but this might cause issues with certain NDEF parses and applications that do not support this record type. Another solution could be to store the raw data as an “Unknown type record (TNF 5)”, as the NFC-NDEF specification recommends that NDEF parsers should store and forward the payload of said record type without any processing, potentially allowing for raw binary data to be written and read.

## 6. License
Copyright (C) 2024 Fournelle Sam

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE X CONSORTIUM BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

Except as contained in this notice, the name of Fournelle Sam shall not be used in advertising or otherwise to promote the sale, use or other dealings in this Software without prior written authorization from Fournelle Sam.
