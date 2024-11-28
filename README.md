# Marmite

<img src="https://github.com/rochacbruno/marmite/raw/main/assets/_resized/logo_160x120.png" align="left" alt="marmite">

Marmite [**Mar**kdown **m**akes s**ite**s] is a **very!** simple static site generator.

[![AGPL License](https://img.shields.io/badge/license-AGPL-blue.svg)](http://www.gnu.org/licenses/agpl-3.0)
[![Crates.io Version](https://img.shields.io/crates/v/marmite)](https://crates.io/crates/marmite)
[![Docs and Demo](https://img.shields.io/badge/docs-demo-blue)](https://rochacbruno.github.io/marmite/)  
  
[![Create blog](https://img.shields.io/badge/CREATE%20YOUR%20BLOG%20WITH%20ONE%20CLICK-20B2AA?style=for-the-badge)](https://github.com/rochacbruno/blog)


> I'm a big user of other SSGs but it is frequently frustrating that it takes so much setup to get started.  
Just having a directory of markdown files and running a single command sounds really useful.  
&mdash; Michael, marmite user.

## How it works

It does **"one"** simple thing only:

- Reads all `.md` files on the `input` directory.
- Using `CommonMark` parse it to `HTML` content.
- Extract optional metadata from `frontmatter` or `filename`.
- Generated `html` file for each page.
- Outputs the rendered static site to the `output` folder.

It also handles generating or copying `static/` and `media/` to the `output` dir.

## Before you start, you should know

1. Marmite is meant to be simple, don't expect complex features
2. Marmite is for **bloggers**, so writing and publishing articles in chronological order is the main use case.
3. The generated static site is a **flat** HTML site, no subpaths, all content is published in extension ending URLS ex: `./{name}.html|rss|json`
4. There are only 2 taxonomies `tags:` (to group similar content together) and `stream:` (to separate content in a different listing) 
5. Marmite uses the `date:` attribute to differentiate `posts` from `pages`

## Features

- Everything embedded in a single binary.
- Zero-Config to get started.
  - optionally fully configurable
- Common-mark + Github Flavoured Markdown + Extensions.
- Raw HTML allowed.
- Emojis `:smile:`, spoiler `||secret||`.
- Wikilinks `[[name|url]]` and Obsidian links `[[page]]`.
- Backlinks.
- Tags.
- Multi authors.
  - Author profile page
- Multi streams.
  - Separate content in different listing
- Pagination.
- Static Search Index.
- RSS Feeds.
  - Multiple feeds (index, tags, authors, streams)
- Built-in HTTP server.
- Auto rebuild when content changes.
- Built-in theme 
  - Light and Dark modes.
  - Multiple colorschemes
  - Fully responsive
  - Spotlight Search.
  - Easy to replace the index page and add custom CSS/JS
  - Easy to customize the templates
  - Math and Mermaid diagrams.
  - Syntax Highlight.
  - Commenting system integration.
  - Banner images and `og:` tags.
- CLI to start a new theme from scratch


## Installation

Install with the install script

```bash
curl -LSfs https://raw.githubusercontent.com/rochacbruno/marmite/main/assets/install.sh | sh
```

Install with cargo

```bash
cargo binstall marmite
```
or

```bash
cargo install marmite
```

Or download the pre-built **binary** from the [releases](https://github.com/rochacbruno/marmite/releases)


<details>

<summary>Or use docker</summary>


> [!IMPORTANT]  
> The directory containing your marmite project must be mapped to containers `/input`  
> If running inside the directory use `$PWD:/input` 
> The result will be generates in a `site` folder inside the input dir.

Build
```console
$ docker run -v $PWD:/input ghcr.io/rochacbruno/marmite
Site generated at: site/
```
Serve (just add port mapping and the --serve)
```console
$ docker run -p 8000:8000 -v $PWD:/input ghcr.io/rochacbruno/marmite --serve
```

> [!INFO]  
> By default will run `:latest`, Add `:x.y.z` with the version you want to run.

</details>

## Usage

It's simple, really!

```console
$ marmite folder_with_markdown_files path_to_generated_site
Site generated at path_to_generated_site/
```

CLI

```console
❯ marmite --help
Marmite is the easiest static site generator.

Usage: marmite [OPTIONS] <INPUT_FOLDER> [OUTPUT_FOLDER]

Arguments:
  <INPUT_FOLDER>   Input folder containing markdown files
  [OUTPUT_FOLDER]  Output folder to generate the site [default:
                   `input_folder/site`]

Options:
  -v, --verbose...
          Verbosity level (0-4) [default: 0 warn] options: -v:
          info,-vv: debug,-vvv: trace,-vvvv: trace all
  -w, --watch
          Detect changes and rebuild the site automatically
      --serve
          Serve the site with a built-in HTTP server
      --bind <BIND>
          Address to bind the server [default: 0.0.0.0:8000]
  -c, --config <CONFIG>
          Path to custom configuration file [default: marmite.yaml]
      --init-templates
          Initialize templates in the project
      --start-theme
          Initialize a theme with templates and static assets
      --generate-config
          Generate the configuration file
      --init-site
          Init a new site with sample content and default configuration
          this will overwrite existing files usually you don't need to
          run this because Marmite can generate a site from any folder
          with markdown files
      --new <NEW>
          Create a new post with the given title and open in the
          default editor
  -e
          Edit the file in the default editor
  -p
          Set the new content as a page
  -t <TAGS>
          Set the tags for the new content tags are comma separated
      --name <NAME>
          Site name [default: "Home" or value from config file]
      --tagline <TAGLINE>
          Site tagline [default: empty or value from config file]
      --url <URL>
          Site url [default: empty or value from config file]
      --footer <FOOTER>
          Site footer [default: from '_footer.md' or config file]
      --language <LANGUAGE>
          Site main language 2 letter code [default: "en" or value from
          config file]
      --pagination <PAGINATION>
          Number of posts per page [default: 10 or value from config
          file]
      --enable-search <ENABLE_SEARCH>
          Enable search [default: false or from config file] [possible
          values: true, false]
      --content-path <CONTENT_PATH>
          Path for content subfolder [default: "content" or value from
          config file] this is the folder where markdown files are
          stored inside `input_folder` no need to change this if your
          markdown files are in `input_folder` directly
      --templates-path <TEMPLATES_PATH>
          Path for templates subfolder [default: "templates" or value
          from config file]
      --static-path <STATIC_PATH>
          Path for static subfolder [default: "static" or value from
          config file]
      --media-path <MEDIA_PATH>
          Path for media subfolder [default: "media" or value from
          config file] this path is relative to the folder where your
          content files are
      --default-date-format <DEFAULT_DATE_FORMAT>
          Default date format [default: "%b %e, %Y" or from config
          file] see
          <https://docs.rs/chrono/0.4.19/chrono/format/strftime/index.html>
      --colorscheme <COLORSCHEME>
          Name of the colorscheme to use [default: "default" or from
          config file] see
          <https://rochacbruno.github.io/marmite/getting-started.html#colorschemes>
      --toc <TOC>
          Show Table of Contents in posts [default: false or from
          config file] this will generate a table of contents for each
          post [possible values: true, false]
      --json-feed <JSON_FEED>
          Generate JSON Feed [default: false or from config file]
          [possible values: true, false]
  -h, --help
          Print help
  -V, --version
          Print version
```

## Getting started

Read a tutorial on how to get started https://rochacbruno.github.io/marmite/getting-started.html and create your blog in minutes.


## Docs 

Read more on how to customize templates, add comments etc on https://rochacbruno.github.io/marmite/ 


## That's all!

**Marmite** is very simple.

If this simplicity does not suit your needs, there are other awesome static site generators.


Here are some that I recommend:

- [Cobalt](https://cobalt-org.github.io/)
- [Zola](https://www.getzola.org/)
- [Zine](https://zineland.github.io/)