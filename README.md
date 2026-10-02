# roller-select

A tiny, dependency-free web component that replaces a dropdown with a single click-to-cycle control. Each click rolls the current label up and out and brings the next option in, so short lists (status, on/off, operators like `+` / `-` / `=`) can be changed with one click instead of open, scan, pick.

Originally built for the [EdgeCart](https://edgecart.io) admin, where it drives product, category, page and option settings.

## Why

A `<select>` costs two clicks and a scan for every change. For lists of two to five options that people flip often, that is friction with no benefit. `<roller-select>` keeps the value visible at all times and changes it in one click, while still submitting like a normal form field.

## Install

Copy `roller-select.js` into your project and load it once:

```html
<script src="roller-select.js"></script>
```

No build step, no framework, no dependencies.

## Usage

```html
<roller-select name="status" value="1">
	<rs-item value="0">Not Active</rs-item>
	<rs-item value="1">Active</rs-item>
	<rs-item value="2">Browse Only</rs-item>
</roller-select>
```

- Each `<rs-item>` is one option. `value` is optional; if left out, the item's text is used.
- The control's `value` attribute sets the starting option. If it is missing or doesn't match, the first item is used.
- Clicking cycles to the next item and wraps back to the first after the last.

### Forms

The component renders a hidden `<input>` carrying the current value, so it submits with any regular `<form>`.

| Attribute on `<roller-select>` | Goes to the hidden input as |
|---|---|
| `name` | `name` |
| `id` | `id` (falls back to `name`) |
| `data-*` | copied as-is |

### Events

Every change fires a `roller-change` event on `document`:

```js
document.addEventListener('roller-change', (e) => {
	const { value, label, source, data } = e.detail;
	// source: the <roller-select> element that changed
	// data:   its data-* attributes, as an object
});
```

Because the event is on `document`, one listener can handle every roller on the page. Use `data-*` attributes to tell them apart, for example `data-id="42"` to save a row with an AJAX call.

### JavaScript API

```js
const roller = document.querySelector('roller-select[name=status]');

roller.value;        // read the current value
roller.value = '2';  // set it (no animation, no event fired)
```

Setting a value that isn't in the list is ignored.

## Styling

The control picks up these CSS custom properties if they are defined, with fallbacks:

| Variable | Used for | Fallback |
|---|---|---|
| `--nc-bg` | background | `#fff` |
| `--nc-border` | border | `#9ca3af` |
| `--nc-text` | text color | `#1a1a1a` |
| `--nc-primary` | hover border | `#2563eb` |

Everything else can be overridden by targeting `roller-select .rs-track` and `roller-select .rs-label` in your own stylesheet.

## Browser support

Any browser with Custom Elements v1 support (all current evergreen browsers).

## License

MIT, see [LICENSE](LICENSE).

## Credits

By Bill in KCMO, for [EdgeCart](https://edgecart.io).
