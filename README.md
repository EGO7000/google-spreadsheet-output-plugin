# google-spreadsheet-output-plugin
Simple vanilla JS (ES6) plugin to display Google Sheets on your site.

- The easiest way if you just need to display a Google spreadsheet on a page on your site
- Using only Google visualization API (gviz) query without authentication
- 2 output ways: flexbox div's and classic table tags (csv output in `<pre>` tag)
- Mobile friendly solution (with limitations)
- Ability to display a range of cells and the result of a query
- Built-in way to output simple formatting inside cells (bold/small/center and line break)
- Table design flexibility through customizable classes

## Demo

You can view a live demo and some examples of how to use the various options [here](https://).

## Getting Started
Compiled code can be found in the `dist` directory. The `src` directory contains development code.

### 0. Share the spreadsheet file with the "Anyone with the link" option and the "Viewer" role.
More information about sharing Google spreadsheet [here](https://support.google.com/drive/answer/2494822?hl=en). Without this setting, the plugin will not work.

### 1. Include a minimal stylesheet at `<head>` section so that the table is displayed correctly.
If you want the table design as in the demo, then include the gss.custom.js file to.
```
<link rel="stylesheet" href="path/to/gss.min.css" />
<link rel="stylesheet" href="path/to/gss.custom.css" />
```

### 2. Include JavaScript on your site.
If you want to use the `stickyHeader` functionality, then you need to include the **[syncscroll](https://github.com/asvd/syncscroll)** library.
```
<script type="text/javascript" src="path/to/syncscroll.js"></script>
<script type="text/javascript" src="path/to/gss.min.js"></script>
```
***Note:*** *`stickyHeader` work only with `outputStyle: 'div'` in config.*

### 3. Add the markup to your HTML where the table will be displayed.
```
<div id="gsTable1"></div>
```
***Note:*** *Plugin does not work with `<div name="gsTable1"></div>` style. It requires IDs.*

### 4. Initialize table output with required set of parameters.
```
<script type="text/javascript">
    let gsTable1 = new GSS({
        divId: 'gsTable1',
        tableId: '44-symbols-from-google-docs-url',
    });
</script>
```

### 5. Profit!

---

## Plugin options
There are 3 input objects that the plugin handles:
```
let params = {
        divId: 'someDivId',
        tableId: '44-symbols-from-google-docs-url',
    };
let config = {};
let locale = {};

let gsTable1 = new GSS(params, config, locale);
```

The minimum required options are indicated in paragraph 4 above.

### Params object options
```
{
    divId: '',   // * required
    tableId: '', // * required, can get after /d/ at url
    gid: '',     // first page is 0 usually, another page can get after gid= at url
    range: '',   // for example: 'A1:C10'
    req: '',     // for example: 'select A, B, C where A="Some text"' or 'select *'
    dataType: 'json',   // json, html, csv - usually no need to edit
                        // 'html' just show table generated direct from server (excluding given config params)
                        // 'csv' will give you output in <pre> tags
}
```

### Config object options
```
{
    outputStyle: 'div', // 'table' or 'div' output
    showEmptyDiv: 0,    // show/hide empty div's
    showHeader: 1,      // show/hide first row
    stickyHeader: 1,    // if true -> showHeader option will ignored
    showFooter: 0,      // decorate last row as tfoot
    divClassWrapper: 'gstable',
    divClassHeader: 'gstable--header',
    divClassFooter: 'gstable--footer',
    divClassRow: 'gstable-row',
    divClassCell: 'gstable-cell',
    emptyCellClass: 'gstable-cell__empty',
}
```

### Locale object options
HTML is supported, but be careful with this!
```
{
    loadingMsg: 'Loading data...',
    loadingError: 'Loading data error',
    emptyTableMsg: 'Table is empty!'
}
```

## Plugin methods
At the moment, the plugin has only one method:
```
let gsTable1 = new GSS(params, config, locale);
    // ...some code
    gsTable1.destroy(); // will clear $divId and remove EventListeners
                        // return $divId
```

---
## Built-in way to output simple formatting inside cells
You can format the output inside a cell with the following constructs:
- `[[ Make bold ]]` transform to `<strong> Make bold </strong>`
- `(( Make small ))` transform to `<small> Make small </small>`
- `{{ Make a centered div }}` transform to `<div style="text-align: center"> Make a centered div </div>`

---

## TODO's / Roadmap / Bug fixes

### TODO's
- Publish to CDN
- Add scroll to right position after reload page
- Put some settings in the config (min-width option and htmlEncode formatting on/off)

### Roadmap
- Completely redesigned demo page
- Implement via ES6 modules + do a Webpack 5 assembly
- Create Vue.js component branch

### Bug fixes
- Fix Safari iOS responsive bug
- Add a check for the existence of the syncscroll function
- Put some settings in the config

---

***The plugin has not been tested in high-load projects, for sure Google will make a limit on a large number of requests to the spreadsheet. However, for small sites and small tables, this solution should work just fine!***
