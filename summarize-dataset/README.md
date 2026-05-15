# Dataset Summarizer

Upload any CSV file and get an instant, structured summary of your data — no code required.

See it in action: [live demo](https://utility-apps.ismvp.org/mercury/summarize-dataset) · [full tutorial](https://runmercury.com/examples/utility-apps/summarize-dataset-app/)

---

## What It Does

This app gives you a quick overview of a dataset the moment you upload it. It's useful at the very start of any data project, when you just want to understand what you're working with before diving into analysis.

After uploading a CSV, you'll see:

- **Basic stats** — row count, column count, number of missing values (with %), number of duplicate rows (with %)
- **Column type breakdown** — how many columns are numeric, categorical, text, datetime, boolean, or constant
- **Data preview** — an interactive table showing the first rows of the dataset, powered by [skrub's TableReport](https://skrub-data.org/)

The app also handles a common real-world problem: date columns stored as plain text. It auto-detects them by checking if the values can be parsed as dates, so they show up in the datetime count even if pandas didn't recognize them automatically.

## How It Works

The notebook uses Mercury widgets to handle the file upload and render the results:

| Widget | Purpose |
|--------|---------|
| `mr.UploadFile` | File picker — accepts `.csv` files up to 1 GB |
| `mr.Indicator` | Displays key metrics as big-number cards |
| `mr.Table` | Shows the column type summary as a formatted table |
| `mr.Markdown` | Renders section headers dynamically |

Column type detection logic:
- **Datetime**: pandas `datetime`/`datetimetz` dtypes, plus any `object` column where >80% of values parse as dates
- **Text**: `object` columns (not datetime) where the average string length exceeds 30 characters
- **Boolean**: pandas `bool` dtype
- **Numeric**: pandas `number` dtype
- **Categorical**: everything that doesn't fall into the above categories
- **Constant**: any column with ≤1 unique value (including NaN)

## Try It Yourself

Grab the notebook, install the dependencies, and see it for yourself.

```bash
pip install mercury pandas skrub
```

Then launch the app — Mercury will open it in your browser automatically.

```bash
mercury summarize-dataset.ipynb
```