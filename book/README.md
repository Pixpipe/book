# How to write a book
Hello, this projects uses [GitBook](https://www.gitbook.com/) and comes with some plugins and a bit of configuration so that you don't have to start from scratch. If you follow these instructions, you will be able to generate a nice looking web-book (such as this one) from a simple-to-maintain list of markdown files.  
Prerequisite:
- you know markdown (otherwise, [read that](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet))
- you have *node* and *npm* (otherwise install all at once [here](https://nodejs.org/en/))
- you want to write a book or some documentation


This book is composed of two folders:
1. `book`, that contains all the original files, written in *markdown*
2. `docs`, where is generated the website corresponding to the book - anything added here will be replaced

It can be convenient to have the generated website in the `docs` folder because GitHub can use it for a GitHub Page.

The *summary* on the left is generated automatically based on the file structure within the `book` folder.

## The Top Level Folder
Just under the folder `book`, you will find several files:
- `book.json`, contains the configuration of the book
- `GLOSSARY.md`, is a sort of dictionary, where present words will be highlighted in the book (hint: hover and wait)
- `README.md`, contains all the content of this welcome page
- `node_modules`, dependencies, don't touch that unless you know what you are doing
- `images`, contains the images that are used globally on the website (top-left logo, favicon, etc.)
- `1-chapter-1`, the content of the first chapter. We'll talk about it

## The Chapter Folders
As you could see, there is a `1-chapter-1` in the same fashion, all your chapters will take this form: a **index** followed by a **name**, and with **spaces** replaced by **dashes**. Inside each chapter, there are *markdown* files, again following the same naming rules as the chapter folders. The `0-README.md` file is mandatory.  
Fortunately, the title displayed in the book are not the name of the files! As you can see on the left column, the title given to the category ("The Chapter One") is the top tile given in the file `0-README.md`. The title of the nested section ("The Part One", "The Part Two") are the title at the top of the corresponding files (Those titles can be totally different as the names of their files).  
In each chapter folder, you can have `images` folder, that will contain images accessible only by pages in this chapter.

## Your Book's Title
In the file `/book/book.json`, you will find a field that looks like that:

```javascript
"title": "My Book",
```
Just replace the `"My Book"` by the title of your choice.

## Your Book's Logo
To modify the logo at the top left of the page, replace the image `book/images/logo.png` by you own (its size doesn't matter, but it will be constrained to the size of the column).  
The same goes for the *favicon*, just replace the image `book/images/favicon.png` by an image of 32x32 pixels.


## Google Analytics
*optional*  
In the file `/book/book.json`, you will find a field that looks like that:

```javascript
"analytics": {
  "google": "UA-XXXXXXXXX-X"
}
```
Just replace the `"UA-XXXXXXXXX-X"` by your own Analytics identifier.

## Generate the website/book
At the root of the project, init the project with:  
```bash
$ npm install
```

Then, you can build with:  
```bash
$ npm run build
```

And you can work in development mode with:
```bash
$ npm run dev
```

The advantage of `dev` over `build` is the constant watch for file modification and dynamic rebuild. In addition, it launches a local server to test your book.
