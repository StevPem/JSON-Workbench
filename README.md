# JSON Workbench

JSON Workbench is a JSON viewer and editor that runs entirely in your browser. The whole app is one HTML file, [`json-workbench.html`](json-workbench.html), with no dependencies, no build step and no server. Your data never leaves your computer.

It is simple enough for a quick look at a file, and has the tools you need for real editing: a tree editor, a code editor that pinpoints errors, a spreadsheet-style table view, JSONPath queries, search, undo and redo, schema generation and CSV export.

## Getting started

1. Download [`json-workbench.html`](json-workbench.html) (on GitHub, open the file and click **Download raw file**).
2. Double-click the file to open it in your browser. Chrome, Edge, Firefox and Safari all work.
3. Load some JSON in any of these ways:
   - Click **Open file** or press `Ctrl+O`.
   - Drag a file onto the window.
   - Paste JSON with `Ctrl+V`.
   - Click **Load sample** to try the app with example data.
   - Choose **Load from URL…** from the New menu. The server must allow cross-origin requests.

The app works offline. It remembers the last document you had open in that browser and restores it the next time you open the file.

> On a Mac, use `Cmd` wherever this guide says `Ctrl`.

## Views

Switch views with the **Tree / Text / Table** buttons in the toolbar, or with `Ctrl+1`, `Ctrl+2` and `Ctrl+3`.

### Tree view

Shows the document as an expandable tree, colored by type: strings, numbers, booleans and null. Closed objects and arrays show their size and a preview of their contents. It stays fast with large files; 50,000 records load in well under a second.

- **Edit a value:** double-click it, or select the row and press `Enter`. In a number, boolean or null field, whatever you type is read as JSON (`42`, `true`, `null`, `{"a": 1}`); anything that isn't valid JSON is stored as text.
- **Rename a key:** double-click the key, or press `F2`.
- **Add items:** the **+** button on a row adds a child to an object or array. `Insert` adds a new item after the selected one. When you add a property, you type the key, press `Enter`, then type the value.
- **Duplicate, delete, move:** use the row buttons, or `Ctrl+D`, `Delete`, and `Alt+↑` / `Alt+↓`.
- **More actions:** right-click a row, or click its **⋯** button, to:
  - change the value's type (string, number, boolean, null, object, array)
  - expand or collapse everything below it
  - sort its keys
  - open it as a table
  - copy its value, path or key
- **Copy and paste:** `Ctrl+C` copies the selected value as JSON. `Ctrl+V` pastes JSON into the selected object or array, or after the selected value.

Text values that look like colors (such as `#2f6fde`) show a small color swatch.

### Text view

A code editor with syntax colouring and line numbers.

- Checks your JSON as you type. An error shows the exact line and column, and that line is highlighted. Click **Go to error** to jump to it.
- **Format** pretty-prints the document and **Minify** compacts it.
- **Repair JSON** fixes common problems: comments, trailing or missing commas, single quotes, unquoted keys, and Python-style `True` / `False` / `None`.
- `Tab` and `Shift+Tab` indent, and `Enter` keeps the current indentation.

### Table view

Shows an array of objects (or the entries of an object) as a spreadsheet.

- Each key becomes a column. Missing values show as a dash.
- Click a column header to sort, and click again to reverse.
- Double-click a cell to edit it. Double-clicking an empty cell adds that key to the row.
- Click a nested `{…}` or `[…]` cell to open it as its own table. Use the path at the top to go back up.
- Double-click a row number to jump to that row in the tree.

## Side panel

Toggle the side panel with the panel button or `Ctrl+B`.

### Inspector

Shows the selected item's path (for example `$.team[2].email`), type, size and depth, with buttons to copy each.

- Edit the selected item as JSON in the editor box. It checks your JSON as you type; press **Apply** or `Ctrl+Enter` to save.
- Text values are edited as plain text, so long and multi-line text is easy to work with.
- Use the **Type** list to convert the value, and the action buttons to add, duplicate, move, sort or delete.

### Query (JSONPath)

Run JSONPath queries against the document. Click a result to jump to it in the tree. You can also **Copy results**, change every match at once with **Set all…**, or remove them with **Delete all**.

| Syntax | Meaning |
|---|---|
| `$` | The root of the document |
| `.key` or `['key']` | A child by name |
| `[0]`, `[-1]` | An array item by index (negative counts from the end) |
| `[1:3]`, `[::-1]` | A slice of an array |
| `*` | Every child |
| `..key` | `key` at any depth |
| `[a,b]` | Several children at once |
| `[?(@.price < 10)]` | A filter. Supports `==` `!=` `<` `<=` `>` `>=`, regex matches (`=~ /^a/i`), `&&`, `\|\|`, `@.items.length`, and existence checks `@.x` and `!@.x` |

Examples:

```text
$.team[*].email
$..[?(@.role == 'Engineer' && @.active)].name
$.team[?(@.skills.length > 2)].name
$.milestones[-1:]
```

## Search

Use the search box in the toolbar, or press `Ctrl+F`, to search keys and values. `Enter` and `Shift+Enter` move between matches, and the tree opens to show each one. **Aa** turns on case-sensitive matching and **.\*** turns on regular expressions. In the Text view, search finds matches in the raw text.

## Tools menu

Each transform applies to the selected item, or to the whole document when the root is selected.

| Tool | What it does |
|---|---|
| Sort keys A→Z (deep) | Sorts object keys alphabetically at every level |
| Sort array by field… | Sorts an array by a field, e.g. `name`, `address.city` or `-price` for descending |
| Remove null values (deep) | Deletes every `null` |
| Parse embedded JSON strings | Turns strings that contain JSON (such as `"{\"a\":1}"`) back into real objects and arrays |
| Flatten / Unflatten | Converts nested objects to dotted keys (`a.b[0].c`) and back |
| Generate JSON Schema | Infers a JSON Schema (draft 2020-12), including types, required fields and formats such as date-time, email, URI and UUID |
| Export array as CSV | Downloads the selected array as a CSV file |
| Copy document (minified) / Copy as escaped string | Copies the document in other forms |
| Expand all / Collapse all | Opens or closes the whole tree |

## Saving

- `Ctrl+S` saves. In Chrome and Edge it writes straight back to the file you opened; in other browsers it downloads a copy.
- `Ctrl+Shift+S` saves under a new name.
- Click the file name in the toolbar to rename the file.
- The **Indent** setting in the status bar (2 spaces, 4 spaces, tab or minified) controls how the file is written and how the Text view is formatted.
- A dot next to the file name means there are unsaved changes.

## Supported input

- `.json` and `.geojson` files
- JSON Lines (`.jsonl` / `.ndjson`), loaded as an array
- Relaxed JSON with comments, trailing commas or single quotes, repaired automatically when you open it
- Invalid JSON opens in the Text view with the error highlighted, so you can fix it

## Keyboard shortcuts

Press `?` in the app to see this list.

| Shortcut | Action |
|---|---|
| `Ctrl+O` / `Ctrl+S` / `Ctrl+Shift+S` | Open / save / save as |
| `Ctrl+Z` / `Ctrl+Y` | Undo / redo (up to 150 steps) |
| `Ctrl+F` | Search |
| `Ctrl+1` / `Ctrl+2` / `Ctrl+3` | Tree / Text / Table view |
| `Ctrl+B` | Toggle side panel |
| `↑` `↓` `←` `→` | Move through the tree, collapse and expand |
| `Enter` / `F2` | Edit value / rename key |
| `Insert` / `Shift+Insert` | Insert after / add a child |
| `Ctrl+D` | Duplicate |
| `Alt+↑` / `Alt+↓` | Move up / down |
| `Delete` | Delete |
| `*` | Expand everything below the selection |
| `Ctrl+Enter` | Apply the Inspector editor |

## Limitations

- Like the browser's own JSON handling, the app moves keys that look like numbers (such as `"2"`) to the front of an object, and integers larger than about 9 quadrillion (2⁵³) lose precision.
- Opening from and saving back to the same file needs a Chromium-based browser (Chrome, Edge, Opera). Other browsers download a copy instead.
- Your last document is stored in your browser's local storage. Private windows and cleared site data lose it, so save your work to a file.
