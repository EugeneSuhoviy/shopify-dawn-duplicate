# Gear Store — Shopify Learning Project Technical Spec

> Dawn-based Shopify store for outdoor gear (Hiking / Climbing / Camping).
> Goal: cover as many real Shopify development topics as possible.

---

## 0. Project Overview

### Concept
Gear Store is an online shop for hiking, climbing, and camping equipment. Three main collections. UI is clean and functional with focus on technical product specs.

### Tech Stack
| Tool | Purpose |
|---|---|
| Shopify CLI | theme pull / dev / push |
| Dawn (GitHub) | Base theme — github.com/Shopify/dawn |
| Shopify Partners | Dev store for development |
| VS Code | Editor + Shopify Liquid extension |
| .shopifyignore | Exclude settings_data.json from push |
| Chrome DevTools | Debug Liquid render, Network tab for Ajax |

### Dawn File Structure
| Folder / File | Contents |
|---|---|
| `layout/theme.liquid` | Main layout — CSS, JS, header/footer sections |
| `sections/` | All theme sections — main page blocks |
| `snippets/` | Reusable pieces — product card, icons, price |
| `templates/` | JSON templates — define which sections appear on each page |
| `assets/` | CSS, JS, theme images |
| `locales/` | Translation files (i18n) |
| `config/settings_schema.json` | Theme settings schema for Theme Editor |
| `config/settings_data.json` | Current settings values (don't commit!) |

---

## 1. Setup

### Task 1.1 — Clone and Run Dawn
**Goal:** Set up local dev environment with Shopify CLI and connect a dev store.

Steps:
1. Create a Dev Store in Shopify Partners → Stores → Add store → Development store
2. Install Shopify CLI: `npm install -g @shopify/cli @shopify/theme`
3. Clone Dawn: `git clone https://github.com/Shopify/dawn.git gear-store`
4. Connect to store: `shopify theme dev --store=your-store.myshopify.com`
5. Verify the theme opens in browser at `localhost:9292`
6. Configure `.shopifyignore`: add `config/settings_data.json`

**What you learn:**
- Shopify CLI workflow (pull / dev / push)
- Dawn folder structure
- `.shopifyignore` — why settings_data.json must not be pushed
- Difference between dev store and production store

---

### Task 1.2 — Populate Store with Content
**Goal:** Create test data to work with all Liquid objects.

What to create in Shopify Admin:
1. 3 collections: Hiking, Climbing, Camping (manual, not automatic)
2. Minimum 12 products — 4 per collection
3. Each product must have:
   - 2+ variants (Size + Color)
   - 3+ images
   - Metafields: `weight_grams` (integer), `material` (text), `temperature_rating` (integer)
   - Tags: `new`, `sale`, or `featured` — for conditional logic
4. 1 Blog "Journal" with 5+ articles, each with 2+ tags
5. 3 static pages: About, FAQ, Contact
6. One test customer account

**Metafields setup:**
Go to Admin → Settings → Custom data → Products and add:
- `weight_grams` — Integer
- `material` — Single line text
- `temperature_rating` — Integer (°C)
- `features` — List of single line text

---

## 2. Layout & Global Elements

### Task 2.1 — Study theme.liquid
**Goal:** Understand how Dawn assembles a page, where CSS/JS are loaded, how global objects are passed.

Study tasks (read and take notes — don't change code):
1. Find where the theme CSS is loaded. How does Dawn use `preload` for critical CSS?
2. Find `render 'header'` and `render 'footer'`. What's the difference between `section` and `render`?
3. How does Dawn pass cart data to JS? Find where `window.shopifyCart` is initialized
4. What are `content_for_header` and `content_for_layout`? Where are they output?
5. Find how `scripts.js` is loaded — `defer` or `async`? Why?

**What you learn:**
- Liquid layout system
- `content_for_header`, `content_for_layout`
- Performance — preload, defer, async
- Global availability of `shop`, `cart`, `settings`

---

### Task 2.2 — Custom Announcement Bar Section
**Goal:** Create a section from scratch — with full schema, blocks, and JS logic.

File: `sections/custom-announcement-bar.liquid`

Features:
- Rotation of multiple messages (via blocks)
- Background and text color settings via schema
- Dismissible bar (× button, save state in sessionStorage)
- Show only to logged in / logged out users (schema toggle)

Schema must include:
```json
blocks: [{ type: "announcement", settings: [
  { type: "text", id: "text" },
  { type: "url", id: "link" }
]}]
settings: [
  { type: "color", id: "background_color" },
  { type: "color", id: "text_color" },
  { type: "select", id: "visibility", options: ["all", "logged_in", "logged_out"] }
]
```

Liquid logic:
- `{% for block in section.blocks %}` to loop messages
- `{% if customer %}` to filter by visibility
- `{{ block.settings.text }}` and `{{ block.settings.link }}`

**What you learn:**
- Section schema — settings + blocks
- Shopify color picker in Theme Editor
- `customer` object for conditional rendering
- `sessionStorage` via vanilla JS

---

## 3. PLP — Product List Page

### Task 3.1 — Study main-collection-product-grid
**Goal:** Understand how Dawn renders the product grid, how pagination and filters work.

Study tasks:
1. Find where the Ajax request fires when a filter changes. What URL is requested?
2. How does Dawn render new products without page reload? Find `renderPage()` in JS
3. What is `collection.filters`? Output to console via Liquid: `{{ collection.filters | json }}`
4. How does pagination work — `paginate` tag, `paginate.pages`, `paginate.current_page`
5. Find `snippets/card-product.liquid` — what object does it accept?

---

### Task 3.2 — Product Card Badges
**Goal:** Modify `snippets/card-product.liquid` — add conditional badges.

Add to `snippets/card-product.liquid`:
- `"NEW"` — if product has tag "new": `{% if card_product.tags contains "new" %}`
- `"SALE"` — if compare_at_price exists: `{% if card_product.compare_at_price > card_product.price %}`
- `"OUT OF STOCK"` — if unavailable: `{% unless card_product.available %}`
- Discount percentage: `{{ card_product.compare_at_price | minus: card_product.price | times: 100.0 | divided_by: card_product.compare_at_price | round }}%`

Badges go through a separate snippet `snippets/custom-badge.liquid` which accepts params via render:
```liquid
{% render 'custom-badge', type: "sale", label: discount_label %}
```

**What you learn:**
- `product.tags contains` — Liquid array check
- Math filters: `minus`, `times`, `divided_by`, `round`
- `unless` tag
- `render` with parameter passing

---

### Task 3.3 — Quick View Modal
**Goal:** Add a Quick View button on the card and a modal window with Ajax-loaded product data.

How it works:
1. "Quick View" button appears on card hover
2. Click → Ajax GET request to `/products/{handle}?view=quick-view`
3. Template `templates/product.quick-view.json` renders a minimal PDP
4. Response is injected into modal via `innerHTML`
5. Modal contains: main image, title, price, variant selector, Add to Cart button

Files to create:
- `templates/product.quick-view.json` — alternate product template
- `sections/product-quick-view.liquid` — minimal PDP for modal
- `assets/custom-quick-view.js` — modal open/close logic

**What you learn:**
- Alternate templates (`product.quick-view.json`)
- `?view=` parameter for template selection
- Fetch API for loading HTML fragments
- Dialog/modal without libraries

---

## 4. PDP — Product Detail Page

### Task 4.1 — Metafields on PDP
**Goal:** Display additional product specs via metafields.

Modify `sections/main-product.liquid`:
1. Add "Specifications" block with a table
2. Output metafields: weight, material, temperature rating
3. Use `unless` to hide empty fields

```liquid
{% assign weight = product.metafields.custom.weight_grams %}
{% unless weight == blank %}
  <tr><td>Weight</td><td>{{ weight.value }}g</td></tr>
{% endunless %}
```

Also add a schema block of type `"specifications"` so the merchant can show/hide the section via Theme Editor.

**What you learn:**
- `product.metafields.namespace.key` syntax
- `.value` for getting metafield value
- `unless` + `blank` check
- Section blocks for visibility control

---

### Task 4.2 — Sticky Add to Cart (Mobile)
**Goal:** Add a sticky panel at the bottom of the screen on mobile with ATC button and current variant.

Files:
- Add HTML in `sections/main-product.liquid` (hidden div `id="sticky-atc"`)
- `assets/custom-sticky-atc.js` — IntersectionObserver to track main ATC visibility

How it works:
1. IntersectionObserver watches the main Add to Cart button
2. When it leaves the viewport — sticky panel appears
3. Sticky panel shows the currently selected variant (name + price)
4. Click on sticky ATC — same submit as the main form
5. Hidden on desktop via CSS media query

**What you learn:**
- IntersectionObserver API
- Syncing state between two UI elements
- `product.selected_or_first_available_variant`
- CSS `position: sticky / fixed`

---

### Task 4.3 — Related Products
**Goal:** Add a related products section via Shopify Product Recommendations API.

File: `sections/custom-related-products.liquid`

How it works:
1. Section renders with an empty container
2. JS makes `GET /recommendations/products.json?product_id={{ product.id }}&limit=4`
3. Returned products are rendered via `render 'card-product'`

Schema:
```json
settings: [{ "type": "range", "id": "products_count", "min": 2, "max": 8, "default": 4 }]
```

**What you learn:**
- Product Recommendations API
- Dynamic card rendering via JS
- `schema type: range`
- `{{ product.id }}` in data attribute for JS

---

## 5. Cart

### Task 5.1 — Study Dawn Ajax Cart
**Goal:** Break down how Dawn implements the Cart Drawer — event system, Cart API, rendering.

Study tasks:
1. Find how `cart-drawer.js` listens for the `cart:open` event. Who dispatches it?
2. What endpoint is used to add a product? What is the request body structure?
3. How is the header counter updated after cart changes?
4. Output the full cart object on a page: `{{ cart | json }}` and study its structure
5. What are `cart.items` and `cart.item_count`? What's the difference between `item.quantity` and `cart.item_count`?

---

### Task 5.2 — Free Shipping Progress Bar
**Goal:** Add a progress bar to the Cart Drawer: "X away from free shipping."

Where to add: `sections/cart-drawer.liquid` — at the top, above the item list

Logic:
1. Free shipping threshold is set in schema (`type: number, id: free_shipping_threshold`)
2. Progress: `cart.total_price / threshold * 100` (calculated in JS)
3. 3 states: "Add X for free shipping" / "Almost there! X left" / "You've got free shipping!"
4. Updates on every cart change via existing cart events

> **Important:** Shopify stores prices in cents. `cart.total_price = 5000` means $50.00. Convert with `| money` or divide by 100 in JS.

**What you learn:**
- `cart.total_price`, `cart.items`
- Shopify money format (cents)
- Section schema in cart-drawer
- Custom events for reactivity

---

### Task 5.3 — Cart Note and Cart Attributes
**Goal:** Add an order note field and a custom attribute (e.g. "Gift wrapping").

Implement in Cart Drawer:
- Textarea for cart note: `<textarea name="note">{{ cart.note }}</textarea>`
- Checkbox "Gift wrapping": `<input type="checkbox" name="attributes[gift_wrapping]" value="yes">`
- Update via `POST /cart/update.js` with the appropriate fields

**What you learn:**
- `cart.note` and `cart.attributes`
- `/cart/update.js` endpoint
- Liquid forms (`form 'cart'`)

---

## 6. Search

### Task 6.1 — Custom Search Results Page
**Goal:** Improve the `/search` page to show results split by type.

Modify `sections/main-search.liquid`:
1. Display separately: Products, Articles, Pages
2. Show result count per type
3. For products — use `card-product` snippet
4. For articles — title + excerpt + date
5. Empty state — "Nothing found" with search suggestions

Liquid objects:
```liquid
search.results        — array of all results
search.results_count  — total count
search.terms          — search query
result.object_type    — "product", "article", "page"
```

**What you learn:**
- `search` object and its properties
- Filter array by type: `| where: "object_type", "product"`
- `highlight` filter: `{{ search.terms | highlight }}`

---

### Task 6.2 — Study Predictive Search
**Goal:** Understand how Dawn implements live search via `/search/suggest.json` API.

Study tasks:
1. Find `predictive-search.js` in Dawn — how does it make requests?
2. What URL is used? What query parameters?
3. How are results rendered — template strings or a separate Liquid file?
4. How is debounce implemented to avoid a request on every keystroke?
5. What happens when Escape is pressed?

---

## 7. Blog

### Task 7.1 — Article List Page
**Goal:** Improve the blog template — add tag filtering and a better layout.

Modify `sections/main-blog.liquid`:
1. Sidebar with all blog tags (filter links)
2. Active tag is highlighted
3. Grid layout — 3 columns on desktop, 1 on mobile
4. Article card: image, title, excerpt, author, date, tags
5. Pagination via `{% paginate blog.articles by 9 %}`

```liquid
{% for tag in blog.all_tags %}
  <a href="{{ blog.url }}/tagged/{{ tag | handle }}"
     class="{% if current_tags contains tag %}active{% endif %}">
    {{ tag }}
  </a>
{% endfor %}
```

**What you learn:**
- `blog.articles`, `blog.all_tags`
- `article` object: `image`, `excerpt`, `author`, `published_at`, `tags`
- `paginate` tag and `paginate` object
- `current_tags` — array of active tag filters
- `| handle` filter

---

### Task 7.2 — Article Page
**Goal:** Improve the article template — add prev/next navigation and related articles.

Modify `sections/main-article.liquid`:
1. Previous / Next article links
2. Related articles — 3 articles with the same tags
3. Estimated reading time: `{{ article.content | strip_html | split: " " | size | divided_by: 200 }} min`
4. Share buttons (Twitter, Facebook, Copy link) — no external libraries

**What you learn:**
- `article.next_article`, `article.previous_article`
- Filter chains: `strip_html` → `split` → `size` → `divided_by`
- `| prepend: shop.url` for absolute URL

---

## 8. Customer Account

### Task 8.1 — Login and Registration Forms
**Goal:** Study Liquid form tags for customer forms, understand validation errors.

Study and modify if needed:
- `templates/customers/login.liquid`
- `templates/customers/register.liquid`
- `templates/customers/reset_password.liquid`

```liquid
{% form 'customer_login' %}
  {{ form.errors | default_errors }}
  <input type="email" name="customer[email]">
  <input type="password" name="customer[password]">
{% endform %}
```

**What you learn:**
- `form` tag for customer forms
- `form.errors` and `default_errors`
- Shopify authentication flow

---

### Task 8.2 — Account Page
**Goal:** Build a full account page with order history and addresses.

Modify `templates/customers/account.liquid`:
1. Tabs or accordion: "Orders" / "Addresses" / "Account Details"
2. Orders table: number, date, status, total, link to details
3. Address list with Edit / Delete buttons
4. Add new address form

```liquid
customer.orders               — array of orders
order.name                    — "#1001"
order.financial_status        — "paid", "pending"
order.fulfillment_status      — "fulfilled", "unfulfilled"
order.total_price | money
customer.addresses            — array of addresses
customer.default_address
```

**What you learn:**
- `customer` object and all its properties
- `order` object and statuses
- Address forms: `form 'customer_address'`, `form 'new_customer_address'`
- Conditional render: `{% if customer %}`

---

## 9. Custom Sections

### Task 9.1 — Hero Banner Section
**Goal:** Create a flexible hero section with full Theme Editor control.

File: `sections/custom-hero.liquid`

Schema settings:
- `image` — image picker
- `mobile_image` — image picker (separate image for mobile)
- `heading` — text
- `subheading` — textarea
- `button_label` — text
- `button_url` — url
- `overlay_opacity` — range (0–80, step 5)
- `text_alignment` — select (left / center / right)
- `min_height` — range (400–900px)
- `color_scheme` — select (light / dark)

**What you learn:**
- All schema setting types: text, textarea, image_picker, url, range, select
- `section.settings.image | image_url: width: 1920 | image_tag`
- Responsive images: `widths` attribute in `image_tag`
- CSS custom properties from Liquid values

---

### Task 9.2 — Testimonials Section
**Goal:** Reviews section — blocks with star rating and avatars, carousel on mobile.

File: `sections/custom-testimonials.liquid`

Block schema (type `"testimonial"`):
- `author_name` — text
- `author_title` — text
- `author_image` — image_picker
- `rating` — select (1 / 2 / 3 / 4 / 5)
- `text` — textarea

Section settings: `heading`, `columns` (select 2/3/4), `enable_carousel_mobile` (checkbox)

Star rating via Liquid:
```liquid
{% assign rating = block.settings.rating | times: 1 %}
{% for i in (1..5) %}
  {% if i <= rating %}★{% else %}☆{% endif %}
{% endfor %}
```

**What you learn:**
- `blocks` in schema — array of editable items
- `{% for block in section.blocks %}` and `{{ block.shopify_attributes }}`
- `| times: 1` to convert string → number
- Numeric loop `for i in (1..5)`

---

### Task 9.3 — Featured Collection Section
**Goal:** Section showing products from a selected collection — with tabs for multiple collections.

File: `sections/custom-featured-collection.liquid`

Block schema (type `"collection"`):
- `collection` — type: collection (picker in Theme Editor)
- `label` — text (tab label)

Section settings: `heading`, `products_per_row` (range 3–5), `rows` (range 1–3)

How it works:
1. Tabs are generated from blocks
2. Active tab shows products from the corresponding collection
3. Switching without page reload — JavaScript + CSS visibility

**What you learn:**
- `type: collection` in block schema
- `block.settings.collection` — collection object
- `block.settings.collection.products` — array of products
- Inline JSON for passing data to JS

---

### Task 9.4 — Custom Footer
**Goal:** Replace the default footer with a custom one with full schema and link menus.

File: `sections/custom-footer.liquid`

Settings: `logo` (image_picker), `tagline` (text), `newsletter_heading` (text), `show_social_icons` (checkbox), `column_1_menu` / `column_2_menu` (type: link_list)

```liquid
{% assign menu = linklists[section.settings.column_1_menu] %}
{% for link in menu.links %}
  <a href="{{ link.url }}">{{ link.title }}</a>
{% endfor %}
```

**What you learn:**
- `type: link_list` in schema
- `linklists` object and `linklist.links`
- `link.url`, `link.title`, `link.active`

---

## 10. Technical Topics

### Task 10.1 — Localization (i18n)
**Goal:** Move all hardcoded strings in Liquid files to `locales/en.json`.

In Dawn, strings are output via `| t` filter:
```liquid
{{ 'products.product.add_to_cart' | t }}
```

Tasks:
1. Find all custom strings in your sections (custom-hero, custom-testimonials, etc.)
2. Move them to `locales/en.json` in proper namespace structure
3. Use `| t` everywhere instead of hardcoded text
4. Add pluralization support: `{{ 'cart.items_count' | t: count: cart.item_count }}`

```json
{
  "sections": { "hero": { "heading_default": "Find Your Gear" } },
  "cart": { "items_count": { "one": "{{count}} item", "other": "{{count}} items" } }
}
```

**What you learn:**
- `| t` filter and namespace structure
- Pluralization in Shopify i18n
- `| t: variable: value` for interpolation

---

### Task 10.2 — Performance
**Goal:** Improve load speed through correct image and resource handling.

Tasks:
1. Check all images in custom sections — are they all using `image_url` + `image_tag`?
2. Add responsive sizes via `widths` parameter:
   ```liquid
   {{ image | image_url: width: 1500 | image_tag: widths: '375, 750, 1100, 1500' }}
   ```
3. For above-the-fold images (hero): `loading: 'eager'`, `fetchpriority: 'high'`
4. For everything else: `loading: 'lazy'`
5. Check via Lighthouse — target: Performance score > 85

**What you learn:**
- `image_url` with `width` parameter — Shopify CDN
- `image_tag` with `widths`, `loading`, `fetchpriority`
- `srcset` generation via Shopify
- Lighthouse audit

---

### Task 10.3 — Custom 404 Page
**Goal:** Create an informative 404 page with search and featured products.

Files: `templates/404.json` + `sections/main-404.liquid`

What to show:
- Large "404" and message
- Embedded search form
- 4 featured products (via `settings` or hardcoded collection)
- Links to main categories

**What you learn:**
- `templates/*.json` structure (Online Store 2.0)
- Standalone section without a specific page binding
- `collections['hiking']` — access collection by handle

---

### Task 10.4 — robots.txt.liquid
**Goal:** Customize `robots.txt` via a Liquid template.

File: `templates/robots.txt.liquid` (special template — `layout: none`)

Tasks:
1. Find Shopify docs on `robots.txt.liquid`
2. Learn what the `default_robots_txt` object is
3. Block `/cart/`, `/checkout/`, `/account/` for all bots
4. Allow Googlebot access to images via `Allow`

**What you learn:**
- Special Liquid templates (robots.txt, sitemap)
- `layout: none` — template without theme.liquid wrapper
- `default_robots_txt` object

---

## 11. AJAX & Storefront API

### Task 11.1 — Cart API: Full Cycle
**Goal:** Write a CartManager class from scratch covering all Cart API endpoints — without using Dawn JS code.

File: `assets/custom-cart-manager.js`

| Endpoint | What it does |
|---|---|
| `GET /cart.js` | Get current cart state (items, total, count) |
| `POST /cart/add.js` | Add item: `{ id: variantId, quantity: 1, properties: {} }` |
| `POST /cart/update.js` | Update multiple items: `{ updates: { variantId: qty } }` |
| `POST /cart/change.js` | Change one item: `{ id, quantity }` or `{ line, quantity }` |
| `POST /cart/clear.js` | Clear the cart entirely |

```js
class CartManager {
  async getCart() { }          // GET /cart.js
  async addItem(variantId, qty, properties) { }
  async updateItems(updates) { }
  async changeItem(id, quantity) { }
  async clearCart() { }
  async getItemCount() { }     // convenience helper
  _dispatch(eventName, data) { } // custom events
}
window.CartManager = new CartManager();
```

Requirements:
- All methods return Promises
- On success — dispatch `cart:updated` custom event with new state
- On error — dispatch `cart:error` with message
- Request headers: `Content-Type: application/json`, `Accept: application/json`
- Test each method via DevTools Console

**What you learn:**
- All Cart API endpoints and their parameters
- `fetch()` with POST, headers, and JSON body
- `CustomEvent` and `dispatchEvent`
- Promise chains and async/await error handling

---

### Task 11.2 — Section Rendering API
**Goal:** Understand how Dawn updates sections without page reload via `?sections=` — and write your own example.

How it works:
```js
fetch("/cart?sections=cart-drawer")
// → { "cart-drawer": "<div id=\"shopify-section-cart-drawer\">...</div>" }
```

Study Dawn:
1. Find where Dawn uses `getSectionsToRender()`
2. How does Dawn immediately re-render cart-drawer after `/cart/add.js`?
3. What is the response structure — object with keys = section names?

Own implementation:
Add a "Add to Wishlist" button on PDP. On click — update the header counter without page reload:
1. Click → save `product.id` in localStorage `wishlist[]`
2. Fetch `/?sections=header` to get fresh header HTML
3. Replace `innerHTML` of the header element
4. Header must show wishlist item count

**What you learn:**
- `?sections=` parameter — key Shopify feature
- Section Rendering API response structure
- `innerHTML` section replacement
- `localStorage` for wishlist state

---

### Task 11.3 — Product JSON & Variant Availability
**Goal:** Load current product and variant data via AJAX instead of hardcoding in Liquid.

```js
GET /products/{handle}.js  // or /products/{handle}.json
// Returns full product object with variants, images, options
```

Tasks:
1. On PDP load — fetch `/products/{{ product.handle }}.js`
2. Store response in `window.productData`
3. On variant selection — get price and `available` from `window.productData.variants`
4. Show "In Stock" / "Out of Stock" dynamically based on `variant.available`
5. If variant unavailable — ATC button disabled + changed text

Also — Collection products JSON:
```js
GET /collections/{handle}/products.json?limit=4&sort_by=best-selling
```
Use this endpoint in Related Products (task 4.3) as an alternative to Recommendations API.

**What you learn:**
- `/products/{handle}.js` response structure
- `variant.available`, `variant.price`, `variant.compare_at_price`
- `/collections/{handle}/products.json` parameters
- Dynamic UI updates based on JS data

---

### Task 11.4 — Predictive Search from Scratch
**Goal:** Write your own predictive search component without using Dawn code.

File: `assets/custom-predictive-search.js`

API endpoint:
```
GET /search/suggest.json?q={query}&resources[type]=product,article,page&resources[limit]=5
// Response: { resources: { results: { products: [], articles: [], pages: [] } } }
```

What to implement:
1. Input with 300ms debounce
2. Minimum 2 characters before searching
3. Dropdown with results: Products (with image + price), Articles
4. Query highlighting via `String.replace` with `<mark>`
5. Keyboard navigation: arrow keys, Enter to navigate, Escape to close
6. Loading state while request is in flight
7. Empty state if no results

```js
class PredictiveSearch {
  constructor(inputEl, dropdownEl) { }
  async search(query) { }       // fetch API
  render(results) { }           // build dropdown HTML
  highlight(text, query) { }    // query highlighting
  debounce(fn, delay) { }       // custom debounce
  handleKeyboard(e) { }         // ArrowUp/Down/Enter/Escape
}
```

**What you learn:**
- `/search/suggest.json` and its parameters
- Debounce pattern via `setTimeout/clearTimeout`
- Keyboard navigation (`keydown`, `ArrowUp/Down`)
- `AbortController` for cancelling previous requests
- ARIA attributes for accessibility

---

## 12. Web Components

> Dawn itself is written using Web Components — `cart-drawer`, `product-form`, `variant-selects` and dozens of others are custom HTML elements. Understanding Web Components = understanding Dawn from the inside.

**What are Web Components?**
A native browser standard for creating custom HTML elements. Three technologies: Custom Elements (registering a new tag), Shadow DOM (isolated DOM), HTML Templates. Shopify/Dawn uses primarily Custom Elements without Shadow DOM.

---

### Task 12.1 — First Custom Element: `<quantity-input>`
**Goal:** Understand the basic Custom Element API — lifecycle callbacks, `connectedCallback`.

Dawn already has `quantity-input.js`. Write it from scratch yourself without looking at Dawn code.

HTML usage (in Liquid):
```html
<quantity-input>
  <button class="decrement">-</button>
  <input type="number" value="1" min="1" max="{{ product.variants.first.inventory_quantity }}">
  <button class="increment">+</button>
</quantity-input>
```

Implement in the class:
1. `connectedCallback()` — find input and buttons, attach events
2. `+` button increases value, not exceeding `max`
3. `-` button decreases value, not below `min`
4. When `min`/`max` is reached — button becomes `disabled`
5. On value change — dispatch custom `quantity:changed` event

```js
class QuantityInput extends HTMLElement {
  connectedCallback() {
    this.input = this.querySelector('input');
    this.querySelector('.increment').addEventListener('click', () => this.increment());
    this.querySelector('.decrement').addEventListener('click', () => this.decrement());
  }
  increment() { }
  decrement() { }
}
customElements.define('quantity-input', QuantityInput);
```

**What you learn:**
- `customElements.define()` — registering a new tag
- `connectedCallback` — called when element is added to DOM
- `this` as reference to the DOM element
- Difference between `connectedCallback` and `constructor`

---

### Task 12.2 — attributeChangedCallback: `<product-badge>`
**Goal:** React to HTML attribute changes — `observedAttributes` and `attributeChangedCallback`.

```html
<product-badge type="sale" value="25"></product-badge>
<product-badge type="new"></product-badge>
<product-badge type="out-of-stock"></product-badge>
```

Implementation:
1. `static get observedAttributes()` — return `["type", "value"]`
2. `attributeChangedCallback(name, oldVal, newVal)` — re-render on change
3. `render()` method — generates `innerHTML` based on `type` and `value`
4. Different styles per type via CSS attribute selectors: `product-badge[type="sale"]`
5. From JS: `badge.setAttribute("type", "sale")` → automatic update

**What you learn:**
- `static observedAttributes` — which attributes to watch
- `attributeChangedCallback(name, oldValue, newValue)`
- CSS attribute selectors for styling custom elements
- Declarative approach: HTML attribute = component state

---

### Task 12.3 — Study Dawn Web Components
**Goal:** Break down how Dawn uses Web Components — find patterns and understand the architecture.

Study and document in your notes:
1. How many custom elements are in Dawn? (search `customElements.define` in `assets/`)
2. How does `cart-drawer.js` extend `HTMLElement`? Which lifecycle methods does it use?
3. What is `product-form` and how does it submit the form via Ajax?
4. How does `variant-selects` listen to changes and update URL, price, image?
5. Does Dawn use Shadow DOM? (hint: almost never)

Write your answers in Notion — this becomes your Dawn architecture reference.

---

### Task 12.4 — `<modal-dialog>` Component
**Goal:** Write a full modal component as a Web Component — with focus trap, Escape closing, and ARIA attributes.

```html
<modal-dialog id="quick-view-modal">
  <div class="modal-overlay"></div>
  <div class="modal-content">
    <button class="modal-close">✕</button>
    <div class="modal-body"></div>
  </div>
</modal-dialog>
```

```js
document.querySelector('#quick-view-modal').open();
document.querySelector('#quick-view-modal').close();
```

Implement:
1. `open()` and `close()` public methods
2. On open: `aria-modal="true"`, `aria-hidden="false"`, focus on first focusable element
3. Escape key closes modal
4. Click on overlay closes modal
5. Focus trap — Tab doesn't leave the modal
6. On close — return focus to the element that triggered the modal
7. Scroll lock on body while modal is open

Use this component in task 3.3 (Quick View) instead of the vanilla JS modal.

**What you learn:**
- Public methods on a Web Component (`open`/`close`)
- Focus trap — `querySelectorAll` focusable elements
- `aria-modal`, `aria-hidden`, `role="dialog"`
- Keyboard event handling (Tab + Shift, Escape)
- `document.body.style.overflow` for scroll lock

---

### Task 12.5 — `<toast-notification>` Component
**Goal:** Lightweight notification component — practice with Shadow DOM and isolated styles.

```js
document.querySelector('toast-notification').show('Added to cart', 'success');
document.querySelector('toast-notification').show('Error', 'error');
```

This is the ONE component where Shadow DOM is used:
1. `constructor()` — `attachShadow({ mode: 'open' })`
2. Styles via `<style>` tag in Shadow DOM — isolated from theme
3. `show(message, type)` — adds toast element, animates entry
4. Auto-dismisses after 3 seconds (or on click)
5. Multiple toasts can show at once — queue

Where to use:
- After successful `/cart/add.js` → "Added to cart!"
- After cart error → "Could not add item"
- After wishlist save → "Saved to wishlist"

**What you learn:**
- `attachShadow({ mode: 'open' })` — Shadow DOM
- `this.shadowRoot` for working with isolated DOM
- CSS animations in Shadow DOM
- Difference between Light DOM and Shadow DOM in practice

---

## 13. Metafields & Metaobjects

> **Metafield** — an extra field on an existing resource (product, collection, variant, shop, etc.).
> **Metaobject** — a completely new entity type with a custom schema you define. Need to add "weight" to a product → metafield. Need a "Team Member" entity with photo, name, and role → metaobject.

---

### Task 13.1 — Metafields: All Data Types
**Goal:** Create and render metafields of different types — understand how type affects Liquid output.

Create in Admin → Settings → Custom data → Products:

| Namespace.key | Type |
|---|---|
| `custom.material` | Single line text |
| `custom.weight_grams` | Integer |
| `custom.temperature_rating` | Integer |
| `custom.care_instructions` | Rich text |
| `custom.product_color` | Color |
| `custom.size_guide_image` | File (Image) |
| `custom.related_guide` | URL |
| `custom.difficulty_rating` | Rating (1–5) |
| `custom.features` | List of single line text |
| `custom.paired_products` | List of product references |

Rendering examples:

**Single line text / Integer:**
```liquid
{{ product.metafields.custom.material.value }}
{{ product.metafields.custom.weight_grams.value }}g
```

**Rich text — use `metafield_tag` for correct HTML:**
```liquid
{{ product.metafields.custom.care_instructions | metafield_tag }}
```

**Color — value returns a hex string:**
```liquid
{% assign color = product.metafields.custom.product_color.value %}
<span style="background: {{ color }}; width:20px; height:20px; display:inline-block;"></span>
```

**File (Image) — value returns MediaImage object:**
```liquid
{% assign guide = product.metafields.custom.size_guide_image.value %}
{% if guide != blank %}
  {{ guide | image_url: width: 800 | image_tag }}
{% endif %}
```

**Rating — value.rating and value.scale_max:**
```liquid
{% assign rating = product.metafields.custom.difficulty_rating.value %}
{% for i in (1..rating.scale_max) %}
  {% if i <= rating.rating %}★{% else %}☆{% endif %}
{% endfor %}
```

**List of single line text — value is an array:**
```liquid
{% for feature in product.metafields.custom.features.value %}
  <li>{{ feature }}</li>
{% endfor %}
```

**List of product references — value is an array of product objects:**
```liquid
{% for paired in product.metafields.custom.paired_products.value %}
  {% render 'card-product', card_product: paired %}
{% endfor %}
```

**What you learn:**
- All metafield types and how they differ in Liquid
- `| metafield_tag` — automatic render based on type
- File reference — how to get image URL
- Rating type — object with `.rating` and `.scale_max`
- List types — iterating over array of values
- Product reference — full product object as a value

---

### Task 13.2 — Metafields: All Resources
**Goal:** Go beyond product metafields — add metafields to collection, variant, article, shop.

**Collection metafields** (Admin → Custom data → Collections):
- `custom.banner_image` — File (Image)
- `custom.collection_description_extended` — Rich text
- `custom.season` — Single line text

```liquid
{% assign banner = collection.metafields.custom.banner_image.value %}
{% if banner != blank %}
  {{ banner | image_url: width: 1920 | image_tag: loading: "eager" }}
{% endif %}
```

**Variant metafields** (Admin → Custom data → Variants):
- `custom.inseam_cm` — Integer

```liquid
{% assign current_variant = product.selected_or_first_available_variant %}
{% if current_variant.metafields.custom.inseam_cm.value %}
  Inseam: {{ current_variant.metafields.custom.inseam_cm.value }} cm
{% endif %}
```

**Shop metafields** (Admin → Custom data → Shop):
- `custom.free_shipping_threshold` — Integer
- `custom.social_proof_text` — Single line text

```liquid
{{ shop.metafields.custom.social_proof_text.value }}
{% assign threshold = shop.metafields.custom.free_shipping_threshold.value %}
```

**Article metafields:**
```liquid
{{ article.metafields.custom.read_time.value }} min read
```

**What you learn:**
- `collection.metafields`, `variant.metafields`, `shop.metafields`, `article.metafields`
- Same `.metafields.namespace.key` syntax for all resources
- `product.selected_or_first_available_variant` for current variant
- `shop` metafields as global theme configuration storage

---

### Task 13.3 — Metafields in Theme Editor
**Goal:** Link a metafield to section settings — give merchants the ability to pick metafield values directly in Theme Editor.

Tasks:
1. In the Hero section (task 9.1) — add `type: "text"` setting with `id: "custom_badge_text"`
2. In Theme Editor connect it to Dynamic Source → `product.metafields.custom.season`
3. Observe how Shopify automatically fills in the metafield value
4. Add a `type: "product"` setting to custom-hero schema — allows selecting a product

```json
{ "type": "product", "id": "featured_product", "label": "Featured product" }
```

```liquid
{% assign fp = section.settings.featured_product %}
{% if fp != blank %}
  <p>{{ fp.title }} — {{ fp.price | money }}</p>
{% endif %}
```

**What you learn:**
- Dynamic Sources in Theme Editor
- `type: "product"` in section schema
- `section.settings.featured_product` as a product object
- Connection between metafields and Theme Editor

---

### Task 13.4 — Metaobjects: Schema and Content
**Goal:** Create a custom data type via Metaobjects — a brand new entity with its own schema.

Create three Metaobject schemas in Admin → Settings → Custom data → Metaobjects:

**Schema "team_member":**
| Field | Type |
|---|---|
| `name` | Single line text |
| `role` | Single line text |
| `bio` | Rich text |
| `photo` | File (Image) |
| `linkedin_url` | URL |

**Schema "faq_item":**
| Field | Type |
|---|---|
| `question` | Single line text |
| `answer` | Rich text |
| `category` | Single line text |

**Schema "brand_feature":**
| Field | Type |
|---|---|
| `icon` | File (Image) |
| `title` | Single line text |
| `description` | Single line text |

After creating schemas — populate with data:
- 4–5 `team_member` entries
- 8–10 `faq_item` entries with different categories
- 4 `brand_feature` entries

---

### Task 13.5 — Metaobjects: Rendering in Theme
**Goal:** Retrieve Metaobject entries in Liquid and render them — three different approaches.

**Approach 1 — Via `shop.metaobjects` (global access):**
```liquid
{% assign team = shop.metaobjects.team_member.values %}
{% for member in team %}
  <div class="team-card">
    {% assign photo = member.photo.value %}
    {{ photo | image_url: width: 400 | image_tag }}
    <h3>{{ member.name.value }}</h3>
    <p>{{ member.role.value }}</p>
    {{ member.bio | metafield_tag }}
  </div>
{% endfor %}
```

**Approach 2 — Via metafield reference on product:**
Add to Products: `custom.brand_features` — List of metaobject references (type `brand_feature`)
```liquid
{% for feature in product.metafields.custom.brand_features.value %}
  {% assign icon = feature.icon.value %}
  {{ icon | image_url: width: 48 | image_tag }}
  <strong>{{ feature.title.value }}</strong>
  <p>{{ feature.description.value }}</p>
{% endfor %}
```

**Approach 3 — Via section schema (metaobject picker):**
```json
{ "type": "metaobject", "id": "faq_item_ref", "label": "FAQ item",
  "metaobject_type": "faq_item" }
```
```liquid
{% for block in section.blocks %}
  {% assign faq = block.settings.faq_item_ref %}
  <details>
    <summary>{{ faq.question.value }}</summary>
    {{ faq.answer | metafield_tag }}
  </details>
{% endfor %}
```

**What you learn:**
- `shop.metaobjects.type_name.values` — access all entries
- `member.field_name.value` — access a field value
- List of metaobject references — array of metaobjects as a metafield
- `type: "metaobject"` in section schema — picker in Theme Editor
- `| metafield_tag` for rich text fields in metaobjects

---

### Task 13.6 — Three Real Pages Using Metaobjects
**Goal:** Build three full pages using Metaobjects — demonstrating the real workflow.

**Page 1 — About / Our Team:**
- Template: `templates/page.about.json`
- Section: `sections/page-about.liquid`
- Data: all `team_member` entries via `shop.metaobjects.team_member.values`
- 3-column grid with photo, name, role, bio
- LinkedIn link if filled in

**Page 2 — FAQ:**
- Template: `templates/page.faq.json`
- Section: `sections/page-faq.liquid`
- Data: `faq_item` via `shop.metaobjects.faq_item.values`
- Group by category: `{% assign categories = faq_items | map: "category" | uniq %}`
- Accordion: native `<details>/<summary>` HTML — no JS needed

**Page 3 — Brand Features in Hero:**
- Add brand_feature icons below the existing Hero Banner (task 9.1)
- 4 icons: free shipping, returns, warranty, support
- Data from `brand_feature` metaobjects

**What you learn:**
- Alternate page templates (`page.about`, `page.faq`)
- `| map` and `| uniq` for unique categories
- Grouping data in Liquid without `group_by`
- `<details>/<summary>` — native accordion
- Full workflow: Admin schema → populate → Liquid render

---

## Task Summary

| # | Task | Covers | Difficulty |
|---|---|---|---|
| 1.1 | Clone and run Dawn | Shopify CLI, theme workflow | Easy |
| 1.2 | Populate store content | Admin, metafields namespace | Easy |
| 2.1 | Study theme.liquid | layout system, content_for_* | Easy |
| 2.2 | Announcement Bar | schema, blocks, customer obj | Medium |
| 3.1 | Study PLP | collection.filters, paginate | Easy |
| 3.2 | Product card badges | tags contains, math filters, render | Easy |
| 3.3 | Quick View modal | alternate templates, ?view=, fetch() | Hard |
| 4.1 | Metafields on PDP | metafields, unless blank | Easy |
| 4.2 | Sticky ATC | IntersectionObserver, variant state | Medium |
| 4.3 | Related Products | Recommendations API, fetch() | Medium |
| 5.1 | Study Cart Drawer | cart obj, Cart API endpoints | Easy |
| 5.2 | Free Shipping Bar | cart.total_price, money cents | Medium |
| 5.3 | Cart Note + Attributes | cart.note, cart.attributes | Easy |
| 6.1 | Search Results page | search obj, \| where, \| highlight | Easy |
| 6.2 | Study Predictive Search | suggest API, debounce | Easy |
| 7.1 | Blog article list | blog.all_tags, paginate, current_tags | Medium |
| 7.2 | Article page | article.next/prev, filter chains | Medium |
| 8.1 | Login/Register forms | form customer_login, form.errors | Easy |
| 8.2 | Account page | customer.orders, order obj, addresses | Medium |
| 9.1 | Hero Banner section | image_picker, range, url, responsive img | Medium |
| 9.2 | Testimonials section | blocks, numeric loop, shopify_attributes | Medium |
| 9.3 | Featured Collection | collection picker, inline JSON | Hard |
| 9.4 | Custom Footer | linklists obj, link.url/title/active | Medium |
| 10.1 | Localization (i18n) | \| t filter, pluralization | Easy |
| 10.2 | Performance | image_url, widths, loading, srcset | Medium |
| 10.3 | Custom 404 | templates/*.json, collections[handle] | Easy |
| 10.4 | robots.txt.liquid | layout: none, default_robots_txt | Easy |
| 11.1 | Cart API full cycle | all Cart API endpoints, fetch, CustomEvent | Medium |
| 11.2 | Section Rendering API | ?sections= param, innerHTML replace | Medium |
| 11.3 | Product JSON + Variants | /products/{handle}.js, variant.available | Medium |
| 11.4 | Predictive Search | /search/suggest.json, debounce, AbortController | Hard |
| 12.1 | quantity-input element | customElements.define, connectedCallback | Easy |
| 12.2 | product-badge element | observedAttributes, attribute selectors | Easy |
| 12.3 | Study Dawn Components | cart-drawer, product-form, variant-selects | Easy |
| 12.4 | modal-dialog component | public methods, focus trap, aria | Hard |
| 12.5 | toast-notification | attachShadow, shadowRoot, CSS isolation | Medium |
| 13.1 | Metafields: all types | metafield types, metafield_tag, file ref | Medium |
| 13.2 | Metafields: all resources | collection/variant/shop.metafields | Easy |
| 13.3 | Metafields in Theme Editor | type: product in schema, Dynamic Sources | Medium |
| 13.4 | Metaobjects: schema | Metaobjects Admin, schema and content | Easy |
| 13.5 | Metaobjects in Liquid | shop.metaobjects, metaobject ref, picker | Medium |
| 13.6 | About / FAQ / Features | alternate templates, map+uniq, details/summary | Hard |

---

## Weekly Plan

| Week | Focus | Tasks | Outcome |
|---|---|---|---|
| 1 | Setup + Dawn | 1.1, 1.2, 2.1 | Running dev store, content |
| 2 | Layout + PLP | 2.2, 3.1, 3.2 | Announcement Bar, badges |
| 3 | PDP + Cart | 4.1, 4.2, 5.1, 5.2 | Metafields, sticky ATC, shipping bar |
| 4 | Search + Blog | 3.3, 6.1, 7.1, 7.2 | Quick View, Search, Blog |
| 5 | Account | 8.1, 8.2 | Full customer flow |
| 6 | Custom Sections | 9.1, 9.2, 9.3 | Hero, Testimonials, Featured |
| 7 | Footer + Tech | 9.4, 10.1–10.4 | Footer, i18n, performance, 404 |
| 8 | AJAX API | 11.1, 11.2, 11.3 | CartManager, Sections API, Product JSON |
| 9 | Web Components | 11.4, 12.1–12.3 | Predictive Search, Custom Elements |
| 10 | Web Components 2 | 12.4, 12.5 | Modal dialog, Toast notification |
| 11 | Metafields | 13.1, 13.2, 13.3 | All types, all resources, Theme Editor |
| 12 | Metaobjects + Final | 13.4, 13.5, 13.6 | Full portfolio project |

---

## Useful Commands

```bash
shopify theme dev --store=your-store.myshopify.com
shopify theme pull
shopify theme push
shopify theme check
```

## Resources

- Docs: https://shopify.dev/docs/themes
- Liquid reference: https://shopify.dev/docs/api/liquid
- Dawn on GitHub: https://github.com/Shopify/dawn
- Cart API: https://shopify.dev/docs/api/ajax/reference/cart
- Section Rendering API: https://shopify.dev/docs/api/section-rendering
