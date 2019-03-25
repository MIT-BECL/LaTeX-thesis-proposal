# TL;DR
Provided are templates for **MIT's Bioengineering Thesis Proposal**. Depending on your comfort, and level of LaTeX fluency there are three ways to approach a document that requires a large amount of labor and planning.

1. Write the entire latex document in one file: [template found here](https://github.com/mrsunny0/LaTeX-thesis-proposal/tree/master/Thesis%20Proposal-1-single_file).
2. Write the document piece-meal where parts of divided into sections that are then assembled into one major latex document: [template found here](https://github.com/mrsunny0/LaTeX-thesis-proposal/tree/master/Thesis%20Proposal-2-input).
3. Modularly break your latex document into compilable parts that each produce their own document, these documents are then assembled into a main document: [template found here](https://github.com/mrsunny0/LaTeX-thesis-proposal/tree/master/Thesis%20Proposal-3-subfiles).

Here are the pros and cons for each strategy

| One main file | Divide into pieces | Divide into **compilable** pieces |
| --- | --- | --- |
| One main file to manage and edit | Multiple files to edit | Multiple files to edit |
| Basic directory and file structure | Basic directory and complex file structure | Complex directory and complex file structure |
| Inevitably, the code will be gigantic | Divide-and-conquer approach, files are more manageable | Divide-and-conquer approach, files are more manageable and can be compiled separately |
| Longer the document, longer the compile time | Despite multiple files, the main document needs to be compiled each time | Each file can be compiled independently, and then assembled when ready |

# Table of Contents
- [TL;DR](#tl-dr)
- [Knowing your way (aka _paths_)](#knowing-your-way--aka--paths--)
  * [Navigation commands](#navigation-commands)
  * [Relative vs. Absolute paths](#relative-vs-absolute-paths)
  * [How LaTeX sees it](#how-latex-sees-it)
- [Different ways of management](#different-ways-of-management)
  * [Single file](#single-file)
  * [Divide-and-conquer](#divide-and-conquer)
  * [File hierarchy](#file-hierarchy)
- [Takeaway](#takeaway)

# Knowing your way (aka _paths_)
When you work on large projects, most likely you will split your work into folders and subfolders with several files sprinkled around. Whatever way you split it, you will inevitably make a file hierarchy, or a tree structure, where you can traverse this structure from folder to folder to access the files you want.

![tree](http://imgur.com/0ztr4.png)

All operating systems (e.g. Windows, Mac, Linux) provide some graphical user interface (GUI) to point-and-click your way to the folders and files you want. However, for programs on your computer, they have to request access to files by citing the _path_ to the file, almost like a mailing address. Paths are the names of folders delimited by either a forward `/` or backslash `\` that start from the root or relative folder to the desired folder (more below). To navigate without a GUI a [command line](https://en.wikipedia.org/wiki/Command-line_interface) has to be used. Example:

```
C:\Users\Public\Documents # for windows systems
```

will take me to the 'Documents' folder starting from the C:\ drive, which in my case, is the root folder.

*note*: directory and folder are synonymous.

## Navigation commands
There are three fundamental commands for traversing a file hierarchy. The first is to do the traversing, and the second is to list and see what folders you can traverse into, and the last is to see your current file path. For windows and Mac/Linux systems the commands are:

* windows
	- `cd <folder>` = traverse into folder
	- `dir` = list folders and files in current directory
	- `cd` = (no arguments) prints the present working directory
* mac/linux
	- `cd <folder>` = traverse into folder
	- `ls` = list folders and files in current directory
	- `pwd` = prints the present working directory

To make windows lives a bit (lot) harder, paths to folders and files are delimited by `\`, however mac/linux and even LaTeX use `/` as path delimiters and will error when seeing the backslash.

## Absolute vs. Relative paths
**Absolute paths** are the exact folders to traverse starting from the root folder. Therefore you will end up at your destination regardless of your starting point. The downside is that absolute paths can be different between computers and users, since your root folders change their names depending on your login username.

**Relative paths** travel to your destination folder starting from where you are located now. Dot notations `.` and `..` mean _this_, and _the folder above_ given your current folder. For example:

```
cd ..\Public\Videos\Movies
```  

will take me from my current folder, go up one folder, and then down to 'Videos' and then 'Movies'. Relative paths are more powerful as they do not need to know your root folder, and can be used on multiple computers _if_ the file structure is the same.

_advanced topic_: Sometimes absolute paths are necessary, however writing the entire path is cumbersome and prone to error. Therefore, parts of a commonly used path, say `C:\Users\Public` can be aliased into a variable say `PUBLIC`. Therefore, whenever PUBLIC is used it automatically calls the full path it represents. So `PUBLIC\additional\folders` will get you to the 'folders' directory. Please refer to online tutorials to manage aliases, as there is no one good tutorial on this.

## How LaTeX sees it
LaTeX uses the `/` notation to delimit folders, and understands the dot notation for relative paths. When LaTeX compiles, it needs to know the main folder under which to compile and assemble its documents. When using Texmaker or another IDE, the file you chose to compile and the folder it lives under is your main folder for that compilation.

Why this is important is because some resources such as external files, figures, and bibliography entries may not live in this folder, but under separate folders elsewhere relative to the main folder. Therefore, within your LaTeX code you need to specify paths to these external files. You may use absolute paths, but since you know your main folder and the file hierarchy of your LaTeX documents, relative imports are easier to use.

**takeaway**: LaTeX uses the `/` notation even if on Windows. With large documents it is recommended to break down you LaTeX code into subfiles, and when needed, into subfolders. Therefore, use `.` and `..` notation for relative paths to access these files.

# Different ways of management

## Single file
The most straightforward and 'Word.docx' minded way of writing is to assemble all your code into a single file. It's easy to manage one file, and everything lives in the same folder, so no need to be conscious of absolute or relative paths.

However, the difference between LaTeX and Word is that LaTeX is not succint, and requires code and symbols to write. Therefore a single document will look like a scroll of text which is hard to edit, search, and debug. There is no rule of thumb, but documents that exceed 10 or more pages, and have intricate styles such as figures, bibliographies, and sections should not use a single file strategy.

## Divide-and-conquer
When writing documents which contain sections or chapters, using the  `\input` or `\include` packages will help break-down large documents into smaller _.tex_ documents. For example, you have a _main.tex_ and a _hello.tex_ file in the same folder. The _main.tex_ has:

```latex
\documentclass[12pt]{article}
\begin{document}
	\input{hello}
\end{document}
```

The _hello.tex_ file has:
```latex
\textbf{HELLO WORLD}
```

When compiling _main.tex_ your output would be:

**HELLO WORLD**

As if the contents of _hello.tex_ were literally copied into _main.tex_. If for some reason the _hello.tex_ lived in the 'HI' folder then the input command would requires

```latex
...
\input{HI/hello}
...
```

and if the folder were someplace else then the appropriate relative path (or absolute; not recommended) would have to be used.

The differences between `\input` and `\include`, as they both help divide your main file.

| input | include |
| --- | --- |
| \inputs can be nested | \include cannot be nested and has to be used in the main compiled document |
| As if the contents were copied directly from the \input file and into the section where it was called | there will be a page leading and trailing the \include'd file (i.e. \clearpage). Therefore, \include is intended for sections and chapters |
| To compile some \input files rather than others, you have to manually delete or comment them out | the `\includeonly{filename1, filename2}` will include the filenames specified and ignore the rest in the document, thereby automating which files to compile |

## File hierarchy
The main issue with `\input` and `\include` is that to see the changes in the subfile you're editing, the _main_ file needs to be compiled, rather than the actual subfile (which may live in another folder altogether). With longer documents, the compile time increases, which you either wait out, or go in manually to comment out \input's or use the command `\includeonly` liberally.

The more advanced breakdown of LaTeX documents is to use a package called _subfiles_. The subfile package allows you to break down a document into _compilable_ pieces where each subfile can be compiled independently from one another. Therefore you can physically and mentally divide and conquer a long document. One reason this may be difficult to set up is that for subfiles to compile, it needs a document class and its associated preamble. To do this include the subfile package in the _main.tex_ file `\usepackage{subfiles}`, and in the subfiles include:

```latex
documentclass[main.tex]{subfiles}
\begin{document}
% contents of subfile
\end{document}
```   

To highlight, the subfile will require a document class and a begin document environment. The subfile package looks at the documentclass argument, which is the path to _main.tex_, and copy its documentclass, giving consistency between the compiled documents. Note that when a subfile is compiled, the main folder directory is actually where the _main.tex_ file lives, *not* the subfile. Therefore, if the subfile needs to use a relative path to import or include external resources it needs to consider the relative path starting from the 'main' folder.

What makes this process complex is if the main document has a document class and preamble that is external, in other words, the _main.tex_ has an \input command or uses a _.sty_ package. If this was said out loud, the subfile refers to the document class of the main file which itself refers to the styles and packages from another file. There is a clever way to go about this; however, it goes beyond the discussion of paths, and can be looked up when you see the template document.

# Takeaway
The strategies proposed are intended to make editing and managing large projects and documents more manageable. To decide which strategy to use:

* [Single file](#single-file): when completing short assignments or small projects
* [Divide-and-conquer](#divide-and-conquer): when you are comfortable with managing multiple files and folders, and the project is still relatively small
* [File hierarchy](#file-hierarchy): when you have mastered the basics of LaTeX, understand how LaTeX compiles and manages resources, and want to draft and edit large projects quickly

There are also many opinionated ways to divide a LaTeX document. Some questions to answer:

* Do you want to keep all of your figures (e.g. .png, .jpg, etc.) in one subfolder
	- or do you want to keep figures in separate chapter/section folders
	- does each chapter/section folder have an accompanying _.tex_ chapter/section file that references these figures?
	- how do you plan to compile these _.tex_ files into your _main.tex_ file?
* Are your bibliography _.bib_ entries in one subfolder or compartmentalized with the _.tex_ file which references it?
	- Are these files themselves in a subfolder separate from the _main.tex_ folders
	- how do you plan to compile these _.tex_ files into your _main.tex_ file?
* Do you want to create tables in separate _.tex_ files and \input them when necessary?
* Preambles can be quite large, do you want to separate it into a _.sty_ file and import it in _main.tex_ instead?

As long as you acheive your desired compiled output, all the power to you. Have fun thinking, and enjoy tex'ing!
