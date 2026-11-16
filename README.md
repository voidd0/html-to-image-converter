# HTML to Image Converter: export to CSV or JSON with Python, Node.js or curl

Render an HTML snippet to a PNG, JPEG or WEBP image at any width, with retina scaling and transparent background. Batch mode: process up to 1000 items per run.

This repository has ready scripts for the [HTML to Image Converter: Convert HTML to PNG, JPG and WEBP](https://apify.com/v0iddo/html-to-image) Actor on Apify. The Actor does the
fetching, paging and cleaning; the scripts here run it with your input and save the results as a CSV or JSON file.

## Quick start

1. Create an Apify account and copy your API token from
   [Console > Settings > Integrations](https://console.apify.com/settings/integrations).
2. Install the client and run the script with the example input:

```bash
pip install "apify-client>=2"
export APIFY_TOKEN=your_token
python run.py input.json -o results.csv
```

Edit `input.json` for your own run. You can also run the Actor in the browser on its
[Apify page](https://apify.com/v0iddo/html-to-image), schedule it there, and connect its output to Google Sheets, Slack, Zapier or Make.

## Input

The example input (`input.json`):

```json
{
  "html": "<div style='font:700 48px sans-serif;padding:40px;background:#1fa37a;color:#fff'>Hello, v&oslash;iddo</div>",
  "width": 600,
  "height": 200
}
```

| Field | Type | What it does |
| --- | --- | --- |
| `html` | string | The HTML document or fragment (UTF-8, up to 2 MB). |
| `width` | integer | Viewport width in CSS px |
| `height` | integer | Viewport height in CSS px; omit for a full-page image that fits the content |
| `format` | string | File format of the result. |
| `quality` | integer | jpeg / webp quality |
| `device_scale_factor` | string | Pixel density (2 = retina) (a number) |
| `transparent` | boolean | Transparent page background (png / webp only) |
| `items` | array | Optional. A list of input objects with the same fields as above; every object is processed in its own right and the top-level fields act as shared defaults. |

## Output

One row per result. A real row from a run of the example input (more in
[`examples/sample-output.json`](examples/sample-output.json)):

```json
{
  "index": 0,
  "file_url": "https://api.apify.com/v2/key-value-stores/r0xL0AMK3gPagpcyD/records/result-001-image.png?signature=v2d3NlB6x1gKJDJILyft",
  "file_key": "result-001-image.png",
  "filename": "image.png",
  "mime": "image/png",
  "bytes": 5844,
  "width": 600,
  "height": 200,
  "format": "png",
  "blocked_requests": 0,
  "truncated": false,
  "credits": 3,
  "ms": 2665
}
```

Fields: `index`, `file_url`, `file_key`, `filename`, `mime`, `bytes`, `width`, `height`, `format`, `blocked_requests`, `truncated`, `credits`, `ms`.

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('v0iddo/html-to-image').call({"html": "<div style='font:700 48px sans-serif;padding:40px;background:#1fa37a;color:#fff'>Hello, v&oslash;iddo</div>", "width": 600, "height": 200});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
```

Full example: [`examples/node/run.mjs`](examples/node/run.mjs).

## Any language: one HTTP request

```bash
curl -X POST "https://api.apify.com/v2/acts/v0iddo~html-to-image/run-sync-get-dataset-items" \
  -H "Authorization: Bearer $APIFY_TOKEN" -H 'Content-Type: application/json' \
  -d '{"html": "<div style='\''font:700 48px sans-serif;padding:40px;background:#1fa37a;color:#fff'\''>Hello, v&oslash;iddo</div>", "width": 600, "height": 200}'
```

The response is the JSON array of results.

## Cost

The Actor is paid per result: **$3.40 per 1,000 results**, nothing per run. A new Apify account gets $5 of platform credit every month.

## License

MIT for the code in this repository. Made by [vøiddo](https://voiddo.com).
