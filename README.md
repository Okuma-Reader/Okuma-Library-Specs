# Okuma-Library

The Okuma-Library is where the titles (series/books/manga...) are stored. Its stucture uses the Okuma-Library Directory Structure. This folder mush be accessible online in order to use it with an instance of Okuma-Reader. It isn't necessary to enable files and directory listing on your web server.

## Okuma-Library Directory Structure Specification

The Okuma-Library Directory Structure is composed of 4 directory levels:

1. Library folder
2. Title folder
3. Volume folder
4. Image folder

Each level has a `index.json` file that contains all the metadata necessary to properly parse and display the library and its titles.

Here is an overview of said directory structure:

```
📁 library
 ├─ index.json
 ├─ 📁 my-awesome-book-series
 |   ├─ index.json
 |   ├─ 📁 volume-1-welcome
 |   │   ├─ index.json
 |   │   ├─ thumbnail.jpg
 |   │   ├─ 📁 small
 |   │   │   ├─ index.json
 |   │   │   ├─ _c_1.jpg
 |   │   │   ├─ _c_2.jpg
 |   │   │   ├─ ...
 |   │   │   ├─ 1.jpg
 |   │   │   ├─ 2.jpg
 |   │   │   ├─ ...
 |   │   │   └─ 142.jpg
 |   │   │
 |   │   ├─ 📁 medium
 |   │   └─ 📁 large
 |   │
 |   ├─ 📁 volume-2-the-void
 |   │   ├─ index.json
 |   │   └─ ...
 |   │
 |   └─ 📁 volume-3-whatever
 |       └─ ...
 |
 └─ 📁 another-title
     ├─ index.json
     └─ ...
```

## General notes

- The root folder can be named however you prefer. In this document we have choosen the name "library".
- The properties in the JSON files can have a default value (see the different tables bellow). If that's the case, the property can be omited completely. When the JSON file will be read, the value of a missing property will be its default one. That also means that any property that doesn't have a default value is required, or else the JSON file will be considered invalid.

## First level: Library folder

```
📁 library
 ├─ index.json
 ├─ 📁 my-awesome-book-series
 └─ 📁 another-title
```

The first layer is composed of folders, one for each "title". The folders are names using a [slug](https://en.wikipedia.org/wiki/Clean_URL#Slug). A slug must only contain lowercase letters, numbers, and dashes. For example, a book titled "_My Awesome Book Series_" could use a slug like "my-awesome-book-series". Because the slug will be used as part of the URL, it's recommended to keep it concise. Furthermore, a slug needs to be unique.

Beside the folders, there is also a `index.json` file which follow the [LibraryIndex](#libraryindex) type:

```json
{
  "version": "2.0",
  "titles": ["my-awesome-book-series", "title-of-book"]
}
```

### LibraryIndex

| Property |     Type     | Default | Description                                                         |
| -------- | :----------: | :-----: | ------------------------------------------------------------------- |
| version  |    string    |         | The version of the index.json file. Currently the version is `2.0`. |
| titles   | string array |         | The list of all the titles' slug present in the library.            |

## Second level: Title folder

```
⮌ library

📁 my-awesome-book-series
 ├─ index.json
 ├─ thumbnail.jpg
 ├─ 📁 volume-1-welcome
 ├─ 📁 volume-2-the-void
 └─ 📁 volume-3-whatever
```

Inside each _Title folder_, there are _Volume folders_. Every title has at least one volume. The volumes are stored in individual folder, which are named using the volume's slug.

There is an optional `thumbnail.jpg` file which can be used as the thumbnail for the title/series. If missing, the first volume's thumbnail will be used instead. This file must always be a `.jpg` file as the image is also used as the thumbnail for the [Open Graph protocol](https://ogp.me/) (the preview you get when sharing the URL on social media or messaging apps).

Finally, there is a `index.json` which follow the [TitleIndex](#titleindex) type:

```json
{
  "version": "2.0",
  "pretitle": "An Okuma production",
  "title": "My awesome Book Series",
  "subtitle": "The complete trilogy",
  "volumes": ["volume-1-welcome", "volume-2-the-void", "volume-3-whatever"],
  "status": "completed",
  "synopsis": "A brief outline or general view, as of a subject or written work; an abstract or a summary...",
  "tags": ["Action", "Sci-Fi"],
  "serialization": "Publishing Company Name",
  "credits": [
    { "name": "John Smith", "role": "story" },
    { "name": "Ben Smith", "role": "illustation" }
  ],
  "links": [
    { "title": "Official website", "url": "https://some-website.com" },
    { "title": "Amazon UK", "url": "https://amazon.co.uk/something" }
  ]
}
```

### TitleIndex

| Property      |            Type             | Default | Description                                                                        |
| ------------- | :-------------------------: | :-----: | ---------------------------------------------------------------------------------- |
| version       |           string            |         | The version of the index.json file. Currently the version is `2.0`.                |
| pretitle      |           string            |   ""    | A piece of text placed before the title.                                           |
| title         |           string            |         | The displayed name for that title. Unlike the slug, it can be any string you want. |
| subtitle      |           string            |   ""    | A piece of text placed after the title.                                            |
| volumes       |        string array         |         | The list of the volumes' slugs.                                                    |
| status        | [TitleStatus](#titlestatus) |   ""    | The current status of the title.                                                   |
| synopsis      |           string            |   ""    | The relatively short summary of the book.                                          |
| tags          |        string array         |   []    | A list of genres/tags to quickly understand what the title is about.               |
| serialization |           string            |   ""    | The name of the publishing company.                                                |
| credits       |   [Credit](#credit) array   |   []    | A list of people involved in the production of this title.                         |
| links         |     [Link](#link) array     |   []    | A list of links related to the title.                                              |

### TitleStatus

TitleStatus is a string which value is `upcoming`, `ongoing`, `completed`, or `cancelled`.

### Credit

Credit is an object with the following properties:

| Property |  Type  | Default | Description             |
| -------- | :----: | :-----: | ----------------------- |
| name     | string |         | The name of the person. |
| role     | string |         | The role of the person. |

### Link

Link is an object with the following properties:

| Property |  Type  | Default | Description            |
| -------- | :----: | :-----: | ---------------------- |
| title    | string |         | The title of the link. |
| url      | string |         | The url of the link.   |

## Third level: Volume folder

```
⮌ library / my-awesome-book-series

📁 volume-1-welcome
 ├─ index.json
 ├─ thumbnail.jpg
 ├─ 📁 small
 ├─ 📁 medium
 └─ 📁 large
```

A _Volume folder_ contains 3 image folders: `small`, `medium`, and `large`. Those correspond to different set of the same images, but with different resolution and quality settings. The `small` images are used in the gallery view, where the user see a preview of all the pages in a volume. The `medium` and `large` options are shown in the reader (depending on which quality the user as selected).

Alongside the image folders, there is one file `thumbnail.jpg` which is used has the thumbnail for the volume. For the same reason as the thumbnail in the volume folder, this file must be a `.jpg` file.

Finally there is a `index.json` which follow the [VolumeIndex](#volumeindex) type:

```json
{
  "version": "2.0",
  "pretitle": "Volume 1",
  "title": "Welcome",
  "subtitle": "",
  "publicationDate": "2022-06-21",
  "type": "manga",
  "fileExtension": ".webp",
  "pageCount": 104,
  "pageOrder": "left to right",
  "numberingStart": 1,
  "languages": ["en-US"],
  "bookmarks": [
    {
      "type": "chapter",
      "name": "",
      "page": 1
    },
    {
      "type": "chapter",
      "name": "",
      "page": 52
    }
  ]
}
```

### `VolumeIndex`

| Property        |            Type             |     Default     | Description                                                                                                                        |
| --------------- | :-------------------------: | :-------------: | ---------------------------------------------------------------------------------------------------------------------------------- |
| version         |           string            |                 | The version of the index.json file. Currently the version is `2.0`.                                                                |
| pretitle        |           string            |       ""        | A piece of text placed before the title.                                                                                           |
| title           |           string            |                 | The displayed name for that volume. Unlike the slug, it can be any string you want.                                                |
| subtitle        |           string            |       ""        | A piece of text placed after the title.                                                                                            |
| publicationDate |        [Date](#date)        |       ""        | The publication date for this volume. The format is YYYY-MM-DD.                                                                    |
| type            |  [VolumeType](#volumetype)  |                 | The type of the volume. See the table bellow for more information about the different types.                                       |
| pageCount       |           number            |                 | The number of pages/images in this volume.                                                                                         |
| pageOrder       |   [PageOrder](#pageorder)   | `left to right` | Controls the page order: `left to right` or `right to left`. This parameter is disgarded for volume types `imageset` or `webtoon`. |
| numberingStart  |           number            |        1        | The page number of the first normal page in the volume. This parameter is disgarded for volume types `imageset` and `webtoon`.     |
| languages       | [Language](#language) array |       []        | The list of languages present in the volume.                                                                                       |
| bookmarks       | [Bookmark](#bookmark) array |       []        | Used to organize a volume, just like bookmarks in a PDF file.                                                                      |

### Date

Date is a string formatted as YYYY-MM-DD.

### VolumeType

VolumeType is a string which value is either `manga`, `book`, `imageset`, or `webtoon`.
Here's a comparaison between the different volume types:

| Volume type | Available filters | Special images | Single-page | Double-page | Vertical scrolling |
| ----------- | ----------------- | -------------- | :---------: | :---------: | :----------------: |
| manga       | All               | All            |     ✔️      |     ✔️      |         ❌         |
| book        | All               | All            |     ✔️      |     ✔️      |         ❌         |
| imageset    | Only drop shadow  | Only thumbnail |     ✔️      |     ❌      |         ❌         |
| webtoon     | Only drop shadow  | Only thumbnail |     ✔️      |     ❌      |         ✔️         |

The difference between `manga` and `book` is purely visual. Manga have a rougher texture and the side pages also look different from a typical book.

### PageOrder

PageOrder is a string which value is either `left to right` or `right to left`.

### Language

Language is a string following the [IETF BCP 47](https://en.wikipedia.org/wiki/IETF_language_tag) language tag format (i.e: `fr-CA`, `en-US`, ...).

### Bookmark

Bookmark is an object with the following properties:

| Property |             Type              | Default | Description                                                                                                         |
| -------- | :---------------------------: | :-----: | ------------------------------------------------------------------------------------------------------------------- |
| type     | [BookmarkType](#bookmarktype) |         | The type of the bookmark. Only the value `chapter` is supported.                                                    |
| name     |            string             |   ""    | The name of the bookmark. If omited, chapter are displayed as "Chapter X", where X is incremented for each chapter. |
| page     |            number             |         | The starting page of the bookmark.                                                                                  |

### `BookmarkType`

BookmarkType is a string which value is `chapter`.

## Fourth level: Image folder

```
⮌ library / my-awesome-book-series / volume-1-welcome

📁 small
 ├─ _c_f.webp
 ├─ _c_s.webp
 ├─ ...
 ├─ 1.webp
 ├─ 2.webp
 ├─ ...
 └─ 142.webp
```

An Image folder contains both normal pages (numbered 1, 2, 3, ...), and images with specific names (images for the cover, dust jacket, and obi).

It also contains a very simple `index.json` file:

```json
{
  "version": "2.0",
  "fileExtension": ".webp"
}
```

### `ImageIndex`

| Property      |              Type               | Default | Description                                                                                               |
| ------------- | :-----------------------------: | :-----: | --------------------------------------------------------------------------------------------------------- |
| version       |             string              |         | The version of the index.json file. Currently the version is `2.0`.                                       |
| fileExtension | [FileExtension](#fileextension) |         | The image file format used for this image set. All images in the folder must use the same file extension. |

### `FileExtension`

BookmarkType is a string which starts with a dot (i.e: `.jpg`, `.webp`, `.png`, ...)

### Normal pages

Normal pages are numbered as 1, 2, 3, ... up to the last page. If possible, all pages should have the same size (or at least the same ratio). An image should correspond to one page, images that display two pages side by side should be split.

### Special images

#### Cover

| File name | Description  |
| --------- | ------------ |
| \_c_f     | Front        |
| \_c_s     | Spine        |
| \_c_b     | Back         |
| \_ci_f    | Inside front |
| \_ci_s    | Inside spine |
| \_ci_b    | Inside back  |

#### Cover flaps (aka French flaps)

| File name | Description  |
| --------- | ------------ |
| \_cf_f    | Front        |
| \_cf_b    | Black        |
| \_cfi_f   | Inside front |
| \_cfi_b   | Inside back  |

#### Dust jacket ([see Wikipedia](https://en.wikipedia.org/wiki/Dust_jacket))

| File name | Description  |
| --------- | ------------ |
| \_j_f     | Front        |
| \_j_s     | Spine        |
| \_j_b     | Black        |
| \_ji_f    | Inside front |
| \_ji_s    | Inside spine |
| \_ji_b    | Inside back  |

#### Obi ([see Wikipedia](<https://en.wikipedia.org/wiki/Obi_(publishing)>))

| File name | Description  |
| --------- | ------------ |
| \_o_f     | Front        |
| \_o_s     | Spine        |
| \_o_b     | Black        |
| \_oi_f    | Inside front |
| \_oi_s    | Inside spine |
| \_oi_b    | Inside back  |
