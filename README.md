# wg (website generator)

simple, unix-y website generator in < 40 LOC (based on [sw](https://github.com/jroimartin/sw))

## usage

	wg [directory]

See [sample/](sample/) for a simple example.

## directory structure

Files and folders that start with an underscore (`_`) are considered drafts, and they are skipped during output.

By placing files in an underscore directory at the root of your folder (`./_`), you can configure `wg`. You can do the following things:

### ./_/wg.conf

This file is run before any pages are processed. Use it for preprocessing, or, more likely, use it to export variables you want to use during processing. For example:

	#!/bin/sh

	export SITE_TITLE="My Awesome Website"
	export SITE_AUTHOR="Jonathan Stoler"

### ./_/ft/ directory

This directory is the "filetype" directory. It contains all file processors.

When `wg` processes files in your directory, it looks to see if there is a file in `./_/ft` with the same name as the extension of the current file. If there is, it runs the script at `./_/ft/[EXTENSION]` with the variable `GET_EXT` set. It then expects the script to return the output file's extension (or it will default to `html`). It then runs the script again, with the current file as an argument, and uses the output of that script to fill in the output file.

For example, `./_/ft/md`:

	#!/bin/sh

	test -n "$GET_EXT" && echo "html" && exit 1

	echo $(markdown "$1")
