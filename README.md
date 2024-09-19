# leptos_form: Derive leptos forms from rust structs

<div align="center">
<!-- CI -->
<img src="https://github.com/tlowerison/leptos_form/actions/workflows/continuous_integration.yml/badge.svg" />
<!-- codecov -->
<img src="https://codecov.io/gh/tlowerison/leptos_form/branch/main/graph/badge.svg" />
<!-- Crates version -->
<a href="https://crates.io/crates/leptos_form">
<img src="https://img.shields.io/crates/v/leptos_form.svg?style=flat-square"
alt="Crates.io version" />
</a>
<!-- Downloads -->
<a href="https://crates.io/crates/leptos_form">
<img src="https://img.shields.io/crates/d/leptos_form.svg?style=flat-square"
alt="Download" />
</a>
<!-- docs.rs docs -->
<a href="https://docs.rs/leptos_form">
<img src="https://img.shields.io/badge/docs-latest-blue.svg?style=flat-square"
alt="docs.rs docs" />
</a>
<a href="https://github.com/rust-secure-code/safety-dance/">
<img src="https://img.shields.io/badge/unsafe-forbidden-success.svg?style=flat-square"
alt="Unsafe Rust forbidden" />
</a>
</div>

## Documentation

* [Docs](https://docs.rs/leptos_form)
* [GitHub repository](https://github.com/tlowerison/leptos_form)
* [Cargo package](https://crates.io/crates/leptos_form)
* Minimum supported Rust version: 1.75.0 or later

## Features

* Automatic form parsing -- focus on how your data is represented and not on how to get it in and out of html
* Easy specification of label and input classes, great for Tailwind integration
* Labels are derived from struct fields and can be given form-wide casing
* DOM layout customization through attributes
* Integration with popular crates

## Crate features

This crate offers the following features, all of which are not activated by default:

- `bigdecimal`: Provides impls for [`BigDecimal`](https://docs.rs/bigdecimal/latest/bigdecimal/struct.BigDecimal.html)
- `cache-local-storage`: Provides support for writing intermediate form data to/from local storage.
- `cache-serde_json`: Provides support for (de)serializing form state as JSON.
- `chrono`: Provides impls for [`DateTime`](https://docs.rs/chrono/latest/chrono/struct.DateTime.html), [`NaiveDate`](https://docs.rs/chrono/latest/chrono/naive/struct.NaiveDate.html), [`NaiveDateTime`](https://docs.rs/chrono/latest/chrono/naive/struct.NaiveDateTime.html)
- `num-bigint`: Provides impls for [`BigInt`](https://docs.rs/num-bigint/latest/num_bigint/struct.BigInt.html) and [`BigUint`](https://docs.rs/num-bigint/latest/num_bigint/struct.BigUint.html)
- `uuid`: Provides impls for [`Uuid`](https://docs.rs/uuid/latest/uuid/struct.Uuid.html)
- `ulid`: Provides impls for [`Ulid`](https://docs.rs/ulid/latest/ulid/struct.Ulid.html)
- `i18n`: Provides the ability to use [`leptos-i18n`](https://docs.rs/leptos_i18n/latest/leptos_i18n/)

## Example

```rust,ignore
mod my_crate {
    use leptos::*;
    use leptos_form::prelude::*;
    use serde::*;

    #[derive(Clone, Debug, Default, Deserialize, Form, Serialize)]
    #[form(
        component(
            action = create_my_data(my_data),
            on_success = |DbMyData { id, .. }, _| view!(<div>{format!("Created {id}")}</div>),
            reset_on_success,
        ),
        label(wrap(class = "my-class", rename_all = "Title Case")),
    )]
    pub struct MyData {
        pub my_name: String,
    }

    #[derive(Clone, Debug, Deserialize, Serialize)]
    pub struct DbMyData {
        pub id: i32,
        pub name: String,
    }

    #[component]
    pub fn MyComponent() -> impl IntoView {
        view! {
            <MyData
                initial={MyData::default()}
                top=|| view!(<input type="button" value="Submit" />)
            />
        }
    }

    #[server]
    async fn create_my_data(my_data: MyData) -> Result<DbMyData, ServerFnError> {
        todo!()
    }
}
```
