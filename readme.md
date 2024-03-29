![Icon](./icon.png)

## 🐍 clip.py

A REPL with Alfred's clipboard history being easily accessible in scripts.

Technically this is a `REP`, since the `L` in REPL is for loop. The output is put into your clipboard, so it's value is preserved, but only as another clipboard item. Any state you create in a previous evaluation is not remembered.

### 📶 Setup

By default the `User Configuration` should be setup correctly if you are using Alfred 5.

	* `💽 Database Path` - The PATH to the `clipboard.alfdb` file.
	* `🗃️ Log Level`  - The level of debugging you want. Defaults to `info`.

## ⌨️ Usage

Try: `clippy>1+1`

Open the prompt by typing `cliprepl>` and you'll be presented with a list of previous clipboard items. Try using this as a standard Python REPL such as `cliprepl>1+1` will give you `2`

### 📋 Clipboard

Try: `clippy>t1+t1`

Most of the results you are seeing are the clipboard contents.

To read the results you'll notice that
	* `title` is a unique identifier, which equates to a hex'd of rowid
		* By hex, I mean that a rowid of `231` would turn into `e7`
	* `subtitle` is the value of the item (try hitting `cmd` for the float)

To access the variables in your command, you'll need to give it a type prefix.

#### Prefixes

	* `t` prefix: (eg `te7`) would be the the value casted as a string.
		* For example `cliprepl> te7+te7` would append the text to itself.
	* `f` prefix: (eg `fe7`) is the variable casted as a float.
		* For example `cliprepl> fe7+fe7` would add the two floats together.

#### Advanced

	* Hitting `<enter>` or `<tab>` would autocomplete into 🎩 Alfred.
	* Hitting `<cmd>` will show you the value casted as a float.
	* Ending your query with a semicolon (`;`) will hide the clipboard results.
	* Add custom functions and imports to the 🧡 Orange "Run Script"

## 🔌 Developers, Developers, Developers

### 🪵 Logs

Logs are routed to stderr, this makes them visible in the debugger.

### 📐 Tests

There are a handful of unit tests that are runnable as well:

	* `clippy>.test`

### 💽 Schema

For reference, here is the schema of the `clipboard.alfdb` file as it's currently known.

* `rowid` : A unique identifier generated by sqlite.
* `item` : Likely the value.
* `ts` : The timestamp, in seconds, when it was captured. Non unique.
* `app` : A short name for the application it was copied from.
* `apppath` : The absolute path of the application it was copied from.
* `dataType` : Enum of `0` meaning text, or `1` meaning an image.
* `dataHash` : If `dataType` is `1` this is a reference to the file on disk.

We do not simply sort by `rowid` but often by `ts` as well (`ts DESC, rowid DESC`) because it's possible for Alfred to update the `ts` in cases where you copy it back to your clipboard.
