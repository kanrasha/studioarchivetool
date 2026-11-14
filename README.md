# Studio Archive Search Tool — Documentation

This ReadMe is for Studio Archive Tool version 5.2.0 (StudioArchiveTool-v5-2-0.html).
This application is built for an archive directory that matches the following setup and format(s).

## How to Use

Import an archive database (unzipped) that follows the structure below. Search to view and export results. If using custom files or editing contents, keep in mind the CSV headers must match the headers below exactly.  

To limit processing power, it is not possible to export the entire list from the Hard Drive section as it contains thousands of lines of arbitrary data, it can only be exported after a search filters the results within reason.

### Definitions
- **Filetrees** - used interchangeably with 'File Trees', a method of viewing drive data line by line while preserving folder hierarchies.
- **Filepaths** - used to identify the line by line transcription of hard drive content screenshots, which are written in terms of their full file path.
- **HD** - means 'Hard Drive', but universally refers to external drives.
- **BU** - means 'Backup'.

## Search Modes

- **Precise `[abc]`:** Exact matching. Faster and precise.
- **Tolerant `[a~c]`:** Adjustable tolerance for misspellings & typos.

## Directory Structure

```text
archive/
├── filetrees/
│   └── *.txt
│
├── hard-drives/
│   ├── master-hd-log.csv
│   ├── filepaths/
│   │   └── *.txt
│   ├── screenshots/
│   │   └── *.jpg, *.jpeg, *.png
│   └── spreadsheets/
│       └── *.csv
│
├── images/
│   ├── *.jpg, *.jpeg, *.png, *.heic, etc.
│   └── image-index/
│       └── image-index.csv
│
└── web-archive/
    ├── web-archive-links.csv
    └── text-list/
        └── *.txt
```

### All Searchable Sources

1. `images/` — All image files referenced in the image-index
    - `image-index.csv` — Itemized list of contents and information visible on photographed elements
2. `master-hd-log.csv` — A cross-reference for related hard drive information (each row is also searchable by Backup ID, name, type and notes)
    - `filepaths/` — Txt files of full file paths of itemized drive contents, listed in rows, with directories ending with `/`
    - `screenshots/` — Screenshots of drive contents named by Backup ID + `_screenshot.jpg`
    - `spreadsheets/` — CSV files of itemized drive contents named by Backup ID + `_spreadsheet.csv`
    - `filetrees.txt` — Command line exports of itemized hard drive contents named by Backup ID + `_filetree.txt`
    - `writtenShowLog.csv` — Show-level notes cross-referenced by Backup ID (HD name, date, artist, show notes)
3. `web-archive/text-list/*.txt` — Parsed page text, one blank-line-separated block per record
    - `web-archive-links.csv` — Required cross-reference mapping each text-list file to its Wayback URL and local HTML copy (see format below)

## Data Categories & Searchable Fields

### 1. Vault Images

Physical media inventory with visual documentation.

Vault index headers (`image-index.csv`):

```text
IMAGE ID, Tape #, Studio, Client, Artist, Date, Title / Track, Type, Notes, Code, Source Image
```

- `IMAGE ID` — Source and object identifier (`####_###`): counter follows elements; Left -> Right, Bottom -> Top
- `Tape #` — Triangle-marked ONLY physical tape number
- `Studio` — ...
- `Client` — ...
- `Artist` — ...
- `Date` — ...
- `Title / Track` — Ex: `[Collection: track1; track2]`
- `Type` — Medium (2 inch tape, A-DAT, etc.)
- `Notes` — ...
- `Code` — Tape code `[session code | channel count | reel number | tape code (M = Master)]`. This may be derived in the return data, not necessarily written on the tape.
- `Source Image` — The image file, which should link to opening the image

### 2. Hard Drive Contents

Digital archive spanning all logged backup drives. Unified by a Backup ID.

Master log headers (`master-hd-log.csv`):

```text
BACKUP ID, PARTITION ID, Image-1, Image-2, Image-3, Source Notes Spreadsheet, Incomplete, HD Name, ALT HD Name, Type, Notes, HD Power Supply, Source Doc, Source Screenshot, Source Filetree, Source Spreadsheet
```

- `BACKUP ID` — Drive identifier (`######-##`)
- `PARTITION ID` — Sub-identifier, tags on partition number and enumerator
- `Image-1` — ...
- `Image-2` — ...
- `Image-3` — ...
- `Source Notes Spreadsheet` — Name of original document referenced for Notes
- `Incomplete` — `TRUE` if data collection is incomplete
- `HD Name` — Primary drive name
- `ALT HD Name` — Alternate drive name (tooltip)
- `Type` — Manufacturer (OWC, Lacie, etc).
- `Notes` & `Source Notes Spreadsheet` — Combined display
- `HD Power Supply` — Identifies the type of power supply required
- `Source Doc` — Reference for documents containing original data
- `Source Screenshot` — Reference for screenshot images containing original data
- `Source Filetree` — Reference for file trees containing original data
- `Source Spreadsheet` — Reference for spreadsheet files containing original data

**File tree search:** Full directory paths from `tree` command output. Returns matched text, parent directory, source Volume and Backup ID derived from source filename.

**Spreadsheet search:** Line by line content from csv files. Returns matched text, row number and Backup ID derived from source filename. Cross references master-hd-log to return Volume.

**Screenshot search:** Line by line content from txt files. Returns matched text, row number and Backup ID derived from source filename. Best formatted where each txt file line is a full filepath. Cross references master-hd-log to return Volume.

#### Written Log

Curated show-level metadata. Returns show notes by association via Backup ID.

Written log headers (`writtenShowLog.csv`):

```text
HD BACKUP ID, BU BACKUP ID, SHOW ID, Act/Opener, Engineer, HD, BU, Show Notes
```

- `HD BACKUP ID` — Primary drive dedicated Backup ID
- `BU BACKUP ID` — Backup drive dedicated Backup ID
- `SHOW ID` — Links to specific show only (this feature not built yet; generated not written)
- `Act/Opener` — ...
- `Engineer` — Engineer's initials
- `HD` — Primary drive name
- `BU` — Backup drive name
- `Show Notes` — Detailed performance info

### 3. Web Archive

Wayback Machine snapshots and txt file lists of information organized per webpage.

Web archive headers (`web-archive-links.csv`):

```text
Source File, Online Link, Offline Link
```

- `Source File` — Text-list filename the record came from
- `Matching Text` — Full text block from parsed file
- `Online Link` — "Wayback Machine" link to the archived URL
- `Offline Link` — Local HTML file and assets (returns file path)

Text-list records whose `Source File` has no row in this CSV still appear in search results, but with empty Online/Offline Link cells.

---

*Authored by Connor Milton*
*miltonc90@gmail.com*
