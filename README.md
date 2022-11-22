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

## First level: Library folder

```
📁 library
 ├─ index.json
 ├─ 📁 my-awesome-book-series
 └─ 📁 another-title
```

The first layer is composed of folders, one for each "title". The folders are names using a [slug](https://en.wikipedia.org/wiki/Clean_URL#Slug). A slug must only contain lowercase letters, numbers, and dashes. For example, a book titled "_My Awesome Book Series_" could use a slug like "my-awesome-book-series". Because the slug will be used as part of the URL, it's recommended to keep it concise. Furthermore, a slug needs to be unique.

Beside the folders, there is also a `index.json` file:

```json
{
  "version": "2.0",
  "titles": ["my-awesome-book-series", "title-of-book"]
}
```

- `version` is the version of the index.json file. Currently the version is `2.0`.
- `titles` is the list of all the slugs present at this level.

## Second level: Title folder

```
⮌ library

📁 my-awesome-book-series
 ├─ index.json
 ├─ 📁 volume-1-welcome
 ├─ 📁 volume-2-the-void
 └─ 📁 volume-3-whatever
```

Inside each _Title folder_, there are _Volume folders_. Every title has at least one volume. The volumes are stored in individual folder, which are named using the volume's slug. There is also a `index.json` file that stored information about the title:

```json
{
  "version": "2.0",
  "pretitle": "An Okuma production",
  "title": "My awesome Book Series",
  "subtitle": "The complete trilogy",
  "status": "completed",
  "published": {
    "startDate": "1982-06-12",
    "endDate": "1990-11-06"
  },
  "synopsis": "A brief outline or general view, as of a subject or written work; an abstract or a summary...",
  "tags": ["Action", "Sci-Fi"],
  "credits": [
    { "name": "John Smith", "role": "story" },
    { "name": "Ben Smith", "role": "illustation" }
  ],
  "serialization": "Publishing Company Name",
  "links": [
    { "title": "Official website", "url": "https://some-website.com" },
    { "title": "Amazon UK", "url": "https://amazon.co.uk/something" }
  ],
  "volumes": ["volume-1-welcome", "volume-2-the-void", "volume-3-whatever"]
}
```

- `version` is the version of the index.json file. Currently the version is `2.0`.
- `pretitle` (optional) is a piece of text placed before the title.
- `title` is the displayed name for that title. Unlike the slug, it can be any string you want.
- `subtitle` (optional) is a piece of text placed after the title.
- `status` (optional) is the current status of the title. The available values are `completed`, `ongoing`, and `cancelled`.
- `published` (optional) gives information about when the title began and ended publication.
  - `startDate` is the date the publication started. The format is YYYY-MM-DD.
  - `endDate` (optional) is the date the publication ended. The format is YYYY-MM-DD.
- `synopsis` is the relatively short summary of the book. It can use HTML tags such as \<br> or \<strong> ...
- `tags` is a list of genres/tags to quickly understand what the title is about.
- `credits` (optional) is a list of people involved in the production of this title.
  - `name` is the name of the person.
  - `role` is the role of the person.
- `serialization` (optional) is a name of the publishing company.
- `links` (optional) is a list of links related to the title.
  - `title` is the name of the link.
  - `url` is the url of the link.
- `volumes` is the list of the volumes' slugs.

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

Alongside the image folders, there is one file `thumbnail.jpg` which is used has the thumbnail for the volume (and the thumbnail of the first volume is used as the series' thumbnail). This file must always be a `.jpg` file as the image is also used as the thumbnail of Open Graph preview (the preview you get when sharing the URL on social media or messaging apps).

Finally there is a `index.json`:

```json
{
  "version": "2.0",
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

- `version` is the version of the index.json file. Currently the version is `2.0`.
- `type` is the type of the volume. Currently available types are: `manga`, `book`, `imageset`, and `webtoon`. See the table bellow for more information about the different types.
- `pageCount` is the number of pages/images in this volume.
- `pageOrder` (optional, default: `left to right`) controls whether the pages are arranged from `left to right` or `right to left`. Most Japanese books are meant to be read from right to left contrary to western books. This parameter is disgarded if the volume `type` is `imageset` or `webtoon`.
- `numberingStart` (optional, default: 1) is the page number of the first normal page in the volume. This parameter is disgarded if the volume `type` is `imageset` or `webtoon`.
- `languages` (optional) is the list of languages present in the volume. The format is the [IETF BCP 47](https://en.wikipedia.org/wiki/IETF_language_tag) language tag format.
- `bookmarks` (optional) are used to organize a volume, just like bookmarks in a PDF file.
  - `type` is the type of the bookmark. Only the value `chapter` is supported.
  - `name` (optional) is the name of the bookmark. If omited, chapter are displayed as "Chapter X", where X is incremented for each chapter.
  - `page` is the starting page of the bookmark.

### Volume types

| Volume type | Available filters | Special images | Single-page | Double-page | Vertical scrolling |
| ----------- | ----------------- | -------------- | :---------: | :---------: | :----------------: |
| manga       | All               | All            |     ✔️      |     ✔️      |         ❌         |
| book        | All               | All            |     ✔️      |     ✔️      |         ❌         |
| imageset    | Only drop shadow  | Only thumbnail |     ✔️      |     ❌      |         ❌         |
| webtoon     | Only drop shadow  | Only thumbnail |     ✔️      |     ❌      |         ✔️         |

The difference between `manga` and `book` is purely visual. Manga have a rougher texture and the side pages also look different from a typical book.

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

- `fileExtension` is the image file format used for this image set. All images in the folder must use the same file extension. The fileExtension value must include the dot at the beginning.

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
