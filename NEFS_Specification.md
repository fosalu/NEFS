# NEFS Specification v.2.0.0

## 1. Definition
NEFS (NFC Exchange File System) is a rudimentary file system for NFC devices. It is based on the NDEF (NFC Data Exchange Format) standard and uses the NDEF message format as a foundation for its entry based, hierarchical file system structure. This allows the NEFS file system to be compatible with every NFC device that supports NDEF storage. Each file system entry is stored as a separate NDEF record of either the “Well Known Type (TNF 1)” or the “Unknown type record (TNF 5)” depending on whether its content is text based or a file’s binary data.

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
 - The data entry consists of a file’s binary data, stored as an “Unknown type record (TNF 5)”.

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
- 0x54 68 69 73 20 74 65 78 74 20 66 69 6C 65 20 69 73 20 63 6F 6E 74 61 69 6E 65 64 20 69 6E 73 69 64 65 20 74 68 65 20 72 6F 6F 74 20 64 69 72 65 63 74 6F 72 79 21
- dMedia
- fImage.jpg
- 0xFF D8 FF DB 00 43 00 85 5C 64 75 64 53 85 75 6C 75 96 8E 85 9E C8 FF D9 C8 B7 B7 C8 FF FF FF F2 FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF DB 00 43 01 8E 96 96 C8 ...
- dDocuments
- fHello world.txt
- 0x48 65 6C 6C 6F 20 77 6F 72 6C 64 21 20 54 68 69 73 20 74 65 78 74 20 66 69 6C 65 20 69 73 20 63 6F 6E 74 61 69 6E 65 64 20 69 6E 20 E2 80 9C 2F 4D 65 64 69 61 2F 44 6F 63 75 6D 65 6E 74 73 2F E2 80 9D 2E
- eDocuments
- eMedia
- NEFSend

There are 3 files and 2 directories contained in this example. The first file “Text.txt” is located inside the root directory of the file system. Also contained in the root directory is the directory “Media”, in which we find the file “Image.jpg” and the directory “Documents”. Contained inside the directory “Documents” is the file “Hello world.txt”, meaning that it’s hierarchical path is “/Media/Documents/Hello world.txt”.

## 5. License
Copyright (C) 2024 Fournelle Sam

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE X CONSORTIUM BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

Except as contained in this notice, the name of Fournelle Sam shall not be used in advertising or otherwise to promote the sale, use or other dealings in this Software without prior written authorization from Fournelle Sam.
