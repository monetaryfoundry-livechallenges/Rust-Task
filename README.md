# 15 minute - timed skill test for a Rust Engineer

## Task

`select_listings` in `rust/src/main.rs` returns the catalog unchanged. Make it print the residential listings a buyer sees when they ask for a modeled yield of at least 6. The array must match the reference query in Step 3.

Do the work in this order.

1. **Keep.** A listing stays when `property_type` is `Residential` and `yield_percent` is at least 6. The bound is closed: 6 stays in. Compare the `yield_percent` already stored on the listing.

2. **Order.** Sort what remains by `yield_percent`, highest first. When two yields are equal, the lower `id` comes first. Both rules belong in the comparator.

   `yield_percent` is an `f64`. `f64` does not implement `Ord`, so `sort_by_key` will not accept it. Use `sort_by`. That sort is stable: `Ordering::Equal` keeps the earlier row, so catalog order will not break the tie for you. Reverse only the yield. `Ordering::reverse` on the whole chain reverses `id` as well, and the higher id sorts first.

3. **Note.** Set `modeled_income_note` on each kept listing to `"{yield_percent}% modeled annual yield"`, using that listing's own `yield_percent`. Format the number the way `Display` prints it. Leave the decimal places as they are: `8.4` stays `8.4`.

Array order matters. Field order inside each object does not.

Leave the existing API unchanged, and do not call it from your program. `main` already reads the catalog.

### Coding style

- Edit only `select_listings`. Leave `main`, `find_catalog`, and `Property` alone. `modeled_income_note` is already on the struct. An empty string is omitted from the JSON, so set it on every kept listing.
- Use the standard library. `sort_by` and `format!` cover this. Do not add a dependency, and do not add a file.
- Run `cargo fmt` before you stop. Keep the style of `rust/src/main.rs`: four-space indent, short names, and the serde attributes already written.

## Steps

### 1. Create your branch from the project root before you change any code:

```bash
git checkout -b John-david-1998-03-01
```

### 2. Run the current API from the project root and leave that terminal open:

```bash
cd backend
npm install
npm run dev
```

The API listens on http://localhost:4000.

### 3. Coding and test

Implement the task above, then run your result from the project root:

```bash
cd rust
cargo run
```

Read the API result in another terminal, from the project root:

```bash
curl.exe "http://localhost:4000/api/properties?type=Residential&minYield=6&sort=yieldPercent&order=desc"
```

Pass if the parsed JSON is the same array, in the same order, as http://localhost:4000/api/properties?type=Residential&minYield=6&sort=yieldPercent&order=desc.
