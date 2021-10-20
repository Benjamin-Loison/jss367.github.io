---
layout: post
title: "Software Customizations"
feature-img: "assets/img/rainbow.jpg"
thumbnail: "assets/img/glass_ball.jpg"
tags: [Linux, Mac, Windows]
---

<b>Table of contents</b>
* TOC
{:toc}

## Sublime

#### Tabs

I don't like the way Sublime handles tabs, so I change it to make it more like Chrome. To do this, you'll need to edit your key bindings. Here's how you do that:

* (Windows or Linux) Preferences -> Key Bindings
* (Mac) Sublime Text -> Preferences -> Key Bindings

Then add this to the file:
```
{ "keys": ["ctrl+tab"], "command": "next_view" },
{ "keys": ["ctrl+shift+tab"], "command": "prev_view" }
```
#### Markdown Files

I don't particularly like the way Sublime handles markdown files by default, so I change them. The exact menu location will depend on your operation system, but they're not too different.
* On a Mac, go into Preferences -> Settings -> Syntax Specific (Mac)
* On Linux, go into Preferences -> Settings - Syntax Specific

![png]({{site.baseurl}}/assets/img/sublime_linux_settings.png)

add this:
```
{
	"color_scheme": "Monokai.sublime-color-scheme",
	"draw_centered": false,
	"line_numbers": true,
	"gutter": true,
}
```
Here's another option:
```
{
	"color_scheme": "Packages/Monokai Extended/Monokai Extended.tmTheme",
	"dictionary": "Packages/Language - English/en_US.dic",
	"ignored_packages":
	[
		"Markdown",
		"Vintage"
	],
	"line_numbers": true,
	"show_full_path": true,
	"spell_check": true,
	"theme": "Adaptive.sublime-theme"
}
```
I always install these extensions:
* [Monokai Extended](https://github.com/jonschlinkert/sublime-monokai-extended)
* [Markdown Extended](https://github.com/jonschlinkert/sublime-markdown-extended)

For theme I do adaptive

Color scheme is monokai-extended

You'll want to make sure you used markdown-extended:
View -> Syntax -> Markdown Extended
but this only applies it to one file.

## Terminator

I customize Terminator to make it more like iTerm2. To customize it, you'll need to right click on the window then select "Preferences". Then go into Keybindings.
Here are some things I like to change:
* close_term
* new_window
* split_horiz
* split_vert

## VSCode

I did the same thing with tabs in VSCode. Here's how to do that:



![png]({{site.baseurl}}/assets/img/vscode_keyboard_shortcuts_windows.png)

The ones you're look for are `workbench.action.nextEditor` and `workbench.action.previousEditor`.