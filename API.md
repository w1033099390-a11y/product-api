# Product Catalogue API

Product data for 387 LED lighting products, in **English (`en`)** and **Greek (`el`)**.
Stock levels and prices are not included (`price` and `price_from` are always `null`).

The API is a set of static JSON files (UTF-8). Every endpoint is a plain `GET` with no authentication.
Replace `{BASE}` with the address where the files are hosted, for example `https://example.com`.

## Endpoints

| Endpoint | Returns |
|---|---|
| `{BASE}/v1/index.json` | Languages, product count, list of endpoints |
| `{BASE}/v1/{lang}/products.json` | All products (summary) |
| `{BASE}/v1/{lang}/products/{id}.json` | One product with full details |
| `{BASE}/v1/{lang}/variants.json` | All variants (SKU / EAN lookup) |
| `{BASE}/v1/links.json` | Source link list → product id / variant id |

`{lang}` is `en` or `el`. Product ids are the same in both languages.

`{BASE}/index.html` is a browsable preview of the catalogue.

## Product list — `/v1/{lang}/products.json`

```json
{
  "lang": "en",
  "count": 152,
  "items": [
    {
      "id": 11439,
      "name": "LED Globe Bulb 15W G95 300º E27 - OSRAM CHIP DURIS E 2835",
      "reference": "G9515WOSR-91350",
      "brand": "OSRAM Chip",
      "image": "https://www.factorled.com/web/image/product.template/11439/image_1920",
      "categories": ["Featured", "LED Bulbs & Lamps", "LED Bulbs by Base Type", "E27 LED Bulbs", "..."],
      "variant_count": 2,
      "models": ["913516", "913523"],
      "price_from": null,
      "currency": "EUR",
      "listed_variant_ids": [19430, 19431],
      "source_url": "https://www.factorled.com/en/11439-led-globe-bulb-15w-g95-300-e27-osram-chip-duris-e-2835.html"
    }
  ]
}
```

## Product details — `/v1/{lang}/products/{id}.json`

| Field | Type | Description |
|---|---|---|
| `id` | number | Product id |
| `lang` | string | `en` or `el` |
| `name` | string | Product name |
| `reference` | string | Main reference code |
| `short_description` | HTML string \| null | Short description |
| `description` | HTML string \| null | Full description |
| `brand` | object \| null | `{ id, name, logo }` |
| `categories` | array | `{ id, name, path[] }` (path is the full category trail) |
| `features` | array | Technical specifications: `{ id, name, value, icon }` (e.g. power, voltage, CRI, IP rating, dimensions, warranty) |
| `options` | array | Selectable attributes: `{ id, name, display_type, values: [{ id, name, color, icon }] }` |
| `variants` | array | See below |
| `images` | array | `{ id, main, url, sizes: { "1920", "1024", "512", "256", "128" } }`. The first image is the main image |
| `videos`, `documentation` | array | Videos and technical documents, when available |
| `master_box_qty` | number \| null | Units per master box |
| `is_new` | boolean | New product flag |
| `published_at` | string \| null | Publication date (ISO 8601) |
| `source_url` | string | Original product page (English) |

### Variant

| Field | Type | Description |
|---|---|---|
| `id` | number | Variant id |
| `model` | string \| null | Our model number (`null` when not set) |
| `reference` | string | SKU |
| `barcode` | string \| null | EAN-13 |
| `is_default` | boolean | Default variant of the product |
| `price` | null | Not published (always `null`) |
| `currency` | string \| null | ISO currency code, e.g. `EUR` |
| `listed` | boolean | Variant is part of the selected product list |
| `dimensions_cm` | object | `{ length, width, height }` in cm |
| `weight_kg` | number \| null | Weight in kg |
| `image` | string \| null | Photo of this specific variant, when the product has one per variant (path relative to `{BASE}`, e.g. `img/900021_1.jpg`). Absent or `null` otherwise; use the product `images` |
| `dimension_image` | string \| null | Dimension / line drawing for this variant (URL or path relative to `{BASE}`), when available |
| `options` | array | `{ option_id, option, value_id, value }`, e.g. `Color temperature: Warm White - 2700K` |

## Variant index — `/v1/{lang}/variants.json`

```json
{ "lang": "en", "count": 195, "items": [
  { "id": 19430, "model": "913516", "product_id": 11439, "reference": "G9515WOSR-91351", "barcode": "8435612957466",
    "name": "LED Globe Bulb 15W G95 300º E27 - OSRAM CHIP DURIS E 2835",
    "options": ["Color temperature: Natural White - 4000K"], "listed": true }
]}
```

## Notes

- Image URLs are absolute and point to the original image server.
- HTML in `short_description` / `description` is the original product content; sanitise it before rendering if required.
- Greek texts are translated from the English source.
