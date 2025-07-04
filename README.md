# wordpress-export-to-markdown

原版已更新到3.0，原作者仓库 https://github.com/lonekorean/wordpress-export-to-markdown

我是在 https://github.com/vectorsss/wordpress-export-to-markdown 这个版本基础上做了一些修改，这个版本增加了sulg 的处理。

我为了匹配hugo stack主题做了一定调整,完美从wordpress迁移到hugo。

1、取消图片images 子目录，图片直接和文章index.md文件存在一个文件夹里，每篇文章一个文件夹。
2、特色图片原标签为CoverImage ，与stack主题不兼容，需改为image，由于1的调整，引用路径保证特色图片显示正常。

我的迁移文件规模如下：

4 "page" posts found.
201 "post" posts found.
18 "wp_global_styles" posts found.
12 "wp_font_family" posts found.
2 "wp_navigation" posts found.
1 "wp_template" posts found.
585 attached images found.
410 images scraped from post body content.

目前已全部迁移到Hugo今天网站：[兜兜爸投资笔记](https://ddbzhou.com),备用网址 [ddb.ac.cn](https://ddb.ac.cn)

-------------------------------------

A script that converts a WordPress export XML file into Markdown files suitable for a static site generator ([Gatsby](https://www.gatsbyjs.org/), [Hugo](https://gohugo.io/), [Jekyll](https://jekyllrb.com/), etc.).

Each post is saved as a separate Markdown file with appropriate frontmatter. Images are also downloaded and saved. Embedded content from YouTube, Twitter, CodePen, etc. is carefully preserved.

![wordpress-export-to-markdown running in a terminal](https://user-images.githubusercontent.com/1245573/72686026-3aa04280-3abe-11ea-92c1-d756a24657dd.gif)

## Quick Start

You'll need:
- [Node.js](https://nodejs.org/) v12.14 or later
- Your [WordPress export file](https://wordpress.org/support/article/tools-export-screen/) (be sure to export "All content" if you want to save images and/or pages)

It is recommended that you drop your WordPress export file into the same directory that you run this script from so it's easy to find.

You can run this script immediately in your terminal with `npx`:

```
npx wordpress-export-to-markdown
```

Or you can clone and run (this makes repeated runs faster and allows you to tinker with the code). After cloning this repo, open your terminal to the package's directory and run:

```
npm install && node index.js
```

Either way you run it, the script will start the wizard. Answer the questions and off you go!

## Command Line

The wizard makes it easy to configure your options, but you can also do so via the command line if you want. For example, the following will give you [Jekyll](https://jekyllrb.com/)-style output in terms of folder structure and filenames.

Using `npx`:

```
npx wordpress-export-to-markdown --post-folders=false --prefix-date=true
```

Using a locally cloned repo:

```
node index.js --post-folders=false --prefix-date=true
```

The wizard will still ask you about any options not specifed on the command line. To skip the wizard entirely and use default values for unspecified options, add `--wizard=false`.

## Options

### Use wizard?

- Argument: `--wizard`
- Type: `boolean`
- Default: `true`

Enable to have the script prompt you for each option. Disable to skip the wizard and use default values for any options not specified via the command line.

### Path to WordPress export file?

- Argument: `--input`
- Type: `file` (as a path string)
- Default: `export.xml`

The path to the WordPress export file that you want to parse. It is recommended that you drop your WordPress export file into the same directory that you run this script from so it's easy to find.

### Path to output folder?

- Argument: `--output`
- Type: `folder` (as a path string)
- Default: `output`

The path to the output directory where Markdown and image files will be saved. If it does not exist, it will be created for you.

### Create year folders?

- Argument: `--year-folders`
- Type: `boolean`
- Default: `false`

Whether or not to organize output files into folders by year.

### Create month folders?

- Argument: `--month-folders`
- Type: `boolean`
- Default: `false`

Whether or not to organize output files into folders by month. You'll probably want to combine this with `--year-folders` to organize files by year then month.

### Create a folder for each post?

- Argument: `--post-folders`
- Type: `boolean`
- Default: `true`

Whether or not to save files and images into post folders.

If `true`, the post slug is used for the folder name and the post's Markdown file is named `index.md`. Each post folder will have its own `/images` folder.

    /first-post
        /images
            potato.png
        index.md
    /second-post
        /images
            carrot.jpg
            celery.jpg
        index.md

If `false`, the post slug is used to name the post's Markdown file. These files will be side-by-side and images will go into a shared `/images` folder.

    /images
        carrot.jpg
        celery.jpg
        potato.png
    first-post.md
    second-post.md

Either way, this can be combined with with `--year-folders` and `--month-folders`, in which case the above output will be organized under the appropriate year and month folders.

### Prefix post folders/files with date?

- Argument: `--prefix-date`
- Type: `boolean`
- Default: `false`

Whether or not to prepend the post date to the post slug when naming a post's folder or file.

If `--post-folders` is `true`, this affects the folder.

    /2019-10-14-first-post
        index.md
    /2019-10-23-second-post
        index.md

If `--post-folders` is `false`, this affects the file.

    2019-10-14-first-post.md
    2019-10-23-second-post.md

### Save images attached to posts?

- Argument: `--save-attached-images`
- Type: `boolean`
- Default: `true`

Whether or not to download and save images attached to posts. Generally speaking, these are images that were uploaded by using **Add Media** or **Set Featured Image** when editing a post in WordPress. Images are saved into `/images`.

### Save images scraped from post body content?

- Argument: `--save-scraped-images`
- Type: `boolean`
- Default: `true`

Whether or not to download and save images scraped from `<img>` tags in post body content. Images are saved into `/images`. The `<img>` tags are updated to point to where the images are saved.

### Include custom post types and pages?

- Argument: `--include-other-types`
- Type: `boolean`
- Default: `false`

Some WordPress sites make use of a `"page"` post type and/or custom post types. Set this to `true` to include these post types in the results. Posts will be organized into post type folders.

## Advanced Settings

You can edit [settings.js](https://github.com/lonekorean/wordpress-export-to-markdown/blob/master/src/settings.js) to tweak advanced settings. This includes things like throttling image downloads or customizing the date format in frontmatter.

You'll need to run the script locally (not using `npx`) to make use of advanced settings.
