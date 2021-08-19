# SinglePage

The origin of this project is the hack that turns a browser tab into a text editor:

[data:text/html, <html contenteditable>](data:text/html, <html contenteditable>)

If you insert that for the URL, it'll turn that browser tab into a big textarea where you can type. You can save by hitting Ctrl-s and saving as an html page. That's a handy little hack when you want to quickly jot down a note, but it doesn't offer much of an editing experience, and all you have is a bunch of html files saved somewhere on your local computer.

I wanted something with low overhead like that (just open a bookmark and you're up and running) but with more features:

- A comfortable editing experience
- Markdown support with note preview
- Links to other notes
- Wiki-style creation of new notes by clicking on links to notes that haven't been created
- Version control
- Automatic syncing on all of my machines
- Ability to customize the look and feel
- Easily extensible for any specific tasks I want to do
- Everything in one html file - no command line or any of that - so it works easily on a Chromebook or mobile device with only a browser

After giving it some thought, I decided it should be straightforward. Here is what I came up with for each of the features.

## Group 1

- A comfortable editing experience
- Markdown support with note preview

EasyMDE provides both of these. All I had to do was add the CDN link and a few lines of code.

## Group 2

- Links to other notes
- Wiki-style creation of new notes by clicking on links to notes that haven't been created

This was handled by using links with trailing `?name=pagename`. For instance, to load a file called foo.md, the link is `[foo](?name=foo)`. A little Javascript on page load reads in the `name` argument and then loads that page.

## Group 3

- Version control
- Automatic syncing on all of my machines

Github has an API that's accessible from a Javascript library called Octokit. When a particular page is loaded, it's downloaded from the repo, put into the EasyMDE editor, and set to preview. If it doesn't exist, a blank EasyMDE editor is opened and it's given the specified name when saved.

This requires internet access, but it offers a simple solution to the difficult problem of keeping pages in sync across machines. The underlying markdown file is pulled from the server when the page is loaded. It's saved to the server when you save your changes. There will never be a time you enter some information on your home laptop and find that you don't have it on your work computer.

The syncing works in the background. The user does not need to know anything about Git. Merge conflicts should not be possible when used as intended.

## Group 4

- Ability to customize the look and feel
- Easily extensible for any specific tasks I want to do

Everything takes place in a single HTML file. You can add whatever CSS you want, like any other HTML file. Similarly, you can use Javascript as you wish to do anything you'd otherwise do with Javascript. One of the frustrating things about TiddlyWiki is that it's a massive, complicated codebase - it's a single HTML file, but with thousands of lines of advanced code. I don't know much about Javascript. I figure out how to do things using DuckDuckGo search. It shows, and there are currently about 70 total lines of code in the files I've provided.

## Group 5

- Everything in one html file - no command line or any of that - so it works easily on a Chromebook or mobile device with only a browser

All of the above can be done in one html file. That means you copy the html file to your computer, load it in the browser, and you're good to go.

# Getting Started

I've provided two implementations of the above ideas. Download the raw html file of your choice to your computer, add the information about your Github repo described below, and load the file into your browser.

It might be slow the first time you load a file. That's because it has to download tons of Javascript from various CDNs. After that it should load fairly quickly. The main lag is in the response of Github to API requests.

**singlewiki.html** This is intended as a plain wiki. You start on the index page and make notes. New pages are created by making links to pages that don't yet exist.

**singledaily.html** This is intended to provide a "daily note" experience. You can use it as a journal, as a workspace, as a way to capture things that come to mind, or whatever. On loading, it opens the current day's page. If the page doesn't exist, it's created. In the future I plan to add buttons to open yesterday's note, an index of all of this month's notes, and an index of all of the previous month's notes. For right now, you can access older notes directly in the Github repo, or you can open them using the `?name=...` syntax.

# Configuration

You need a personal access token. That can be created in > Settings > Developer settings > Personal access tokens inside your Github account. Github provides documentation if you're not familiar with this.

You need your Github username and the name of the repo. I assume you can handle that even if you've never used Github before.

Fill in those details inside the html file you downloaded. The relevant lines are

```
var repo_token = "";
var repo_owner = "";
var repo_name = "";
```

Under no circumstances do you want anyone else to view your token. That's more or less the same as giving them the password to your Github account. I strongly discourage you from pushing the token to a Git repo even if it's private.




