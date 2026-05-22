# Excel Cleaner
Upload any Excel file, clean it with one click, and download the result as a CSV — no formulas, no macros required.

See it in action: [live demo](https://utility-apps.ismvp.org/mercury/excel-cleaner) · [full tutorial](https://runmercury.com/examples/utility-apps/excel-cleaner-app/)

---

## What It Does

This app gives you a fast way to inspect and tidy up Excel workbooks without opening Excel. It's useful when you receive a messy `.xlsx` file from someone else and need a clean CSV to feed into your own pipeline.

After uploading an `.xlsx` file, you'll see:

- **Sheets overview** — a table listing every sheet in the workbook with row count, column count, empty cells, and duplicate rows
- **Sheet selector** — a dropdown to pick which sheet to inspect and clean
- **Original data preview** — the raw content of the chosen sheet, exactly as it was in Excel
- **Cleaning options** — checkboxes in the sidebar to normalize column names and/or remove duplicate rows
- **Results table** — the cleaned data, updated live as you toggle options
- **Download button** — exports the cleaned sheet as a CSV file

The app also handles a common real-world annoyance: messy column headers. Names like `"Customer Name (€)"` or `"  Order Date  "` get normalized to `customer_name` and `order_date` — lowercased, accent-stripped, and safe to use in any downstream code.

## How It Works

The notebook uses Mercury widgets to handle the upload, sheet selection, cleaning toggles, and CSV export:

| Widget | Purpose |
|--------|---------|
| `mr.UploadFile` | File picker — accepts `.xlsx` files |
| `mr.Table` | Renders the sheets overview, the original data, and the cleaned results |
| `mr.Select` | Dropdown for choosing which sheet to work on |
| `mr.CheckBox` | Box-style toggles in the sidebar for each cleaning operation |
| `mr.Markdown` | Renders section headers dynamically |
| `mr.Download` | Streams the cleaned sheet as a CSV file |

Cleaning operations:

- **Normalize column names** — converts to lowercase, strips accents, replaces non-alphanumeric characters with `_`, trims leading/trailing underscores
- **Remove duplicate rows** — drops exact row duplicates using pandas `drop_duplicates`

A few details worth knowing:

- Each cleaning checkbox **disables itself once applied**, so you can't accidentally toggle an operation off mid-pipeline
- Checkboxes **reset when you switch sheets**, so cleaning state doesn't leak between sheets
- The downloaded filename is built from the normalized sheet name, so a sheet called `"Q1 Sales 2024"` downloads as `q1_sales_2024-edited.csv`

## Try It Yourself

Grab the notebook, install the dependencies, and see it for yourself.

```bash
pip install mercury pandas openpyxl
```

Then launch the app — Mercury will open it in your browser automatically.

```bash
mercury excel-cleaner.ipynb
```