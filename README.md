# Creating PDF technical reports using Quarto

A custom template for creating PDF technical reports for the National Biodiversity Assessment. This repository contains the basic setup for using Quarto to create technical reports, which you can adapt and customise to your content. An example of a report created using this template can be found at [https://doi.org/10.6084/m9.figshare.30135625](https://figshare.com/articles/online_resource/Technical_Report/30135625?file=58824784).

## Enabling PDF outputs in Quarto

Make sure that you have read the [Quarto documentation for PDF outputs](https://quarto.org/docs/output-formats/pdf-basics.html). Quarto uses [LaTeX](https://www.latex-project.org/) to generate PDF documents, but it is not installed by default in RStudio. The recommended PDF engine is TinyTex, which you can install using the Terminal in RStudio:

``` {.bash filename="Terminal"}
quarto install tinytex
```

## Steps for creating a report from this template

### Front page

The report's front page is created using the [Quarto titlepages extension](https://nmfs-opensci.github.io/quarto_titlepages/). The extension is included in this repo, and the basic elements for the front page is set up in the project's `_quarto.yml` file. The section under `titlepage-pdf` controls the layout of the front page.

``` {filename="_quarto.yml"}
format:
  titlepage-pdf:
    titlepage: formal
    titlepage-logo: "imgs/sanbi-logo-medium.png"
    titlepage-footer: | # text
      National Biodiversity Assessment\
      Technical Reports\
      South African National Biodiversity Institute\
      Pretoria
    titlepage-theme:
      elements: ["\\titleblock", "\\authorblock", "\\vfill", "\\logoblock", "\\footerblock"]
    documentclass: scrbook
    classoption: ["oneside", "open=any"]
```

Do not change any of these settings.

#### Customizing the front page

The report's title and authors need to be added to `_quarto.yml` to enable their appearance on the front page. Look for the following lines in \_quarto.yml and edit as necessary:

``` {filename="_quarto.yml"}
book:
  title: "Report title"
  author: "Report author"
```

### Table of contents

Each chapter or section of your report should be in a separate .qmd file. Sections and subsections are automatically numbered, and only the titles of .qmd pages (header level 1) receive top level numbers. If there are any sections that you don't want numbered, use `{.unnumbered}` after the header. The report's table of contents is automatically generated from the first and second level headers in the .qmd files listed in `_quarto.yml`. List all the .qmd files that form part of your report under the section `chapters:` in `_quarto.yml` :

``` {filename="_quarto.yml"}
 book:
  chapters:
    - index.qmd
    - references.qmd
```

The first page of the report must be named `index.qmd`. Use this page for front matter - recommended sections to include here are:

**Executive summary**

**Acknowledgements**

**Recommended citation**

### Citing literature

The NBA's reference style file is included here (`sanbi.csl`) to use in the compilation of a reference list at the end of the report. Please do not use any other styles. Cited references are automatically collected in `references.bib` if you use Insert \> Citation in RStudio's Visual mode. The file `references.qmd` is already set up to compile the list of cited references, and should be placed last in the chapters list.

### Compiling content

Most Quarto functionality such as cross-referencing sections, figures and tables also work in PDF format. Consult the [Quarto documentation](https://quarto.org/docs/guide/) where necessary. Quarto documentation recommends avoiding underscores in cross-reference labels, e.g. `#fig-figure_1` as this can cause problems when rendering to PDF with LaTex.

Quarto's default format for cross-referencing sections in PDF documents is to label them as 'Chapter'. For example, 'see `@sec-introduction` ...' will be rendered as 'see Chapter 1 ...'. If you prefer it to be labelled as sections instead, use `[section @sec-introduction]` instead.

#### A note on the placement of figures and tables

This template is set up to force Quarto to insert tables and figures in the rendered content where they are placed within the text. However, this only works if there is enough space on the page to contain the table or figure and its caption. If there is not enough space, Quarto will place the table or figure at the end of the .qmd file when rendering. Reducing the height and width of figures and tables, and moving them to a place in the text where there is enough space to fit it onto the page usually solves this problem.

## Rendering the report to PDF

The report can be rendered using the Render button in RStudio, or by typing `quarto render` into RStudio's terminal. This will create a folder labelled `_book` in your project folder, which will contain the pdf version of your report.
