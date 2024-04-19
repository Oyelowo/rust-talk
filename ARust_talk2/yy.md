### Solutions and Features

Surreal ORM was crafted to revolutionize working with databases in Rust. As a solo developer, over 17 intense months beginning December 2022, I developed this tool from a personal frustration with existing solutions. My goal was to dramatically enhance developer productivity and minimize bugs by offering robust, innovative tools. Here's what sets Surreal ORM apart:

- **Statically Checked Raw Queries with no need for a database connection**: All queries are checked at compile time, minimizing runtime errors and enhancing code reliability.
    ![Statically Checked Raw Queries](https://raw.githubusercontent.com/Oyelowo/rust-talk/a9b986e5b8dad179d8968dd461e088ba87f8e142/RUST_TALK/static_check_raw_query.png)
    ![Multiple Queries Checked](https://raw.githubusercontent.com/Oyelowo/rust-talk/a9b986e5b8dad179d8968dd461e088ba87f8e142/RUST_TALK/statically_checked_raw_multiple_queries.png)

- **Intuitive API for Complex Queries**: The API is designed to simplify your database interactions, even as query complexity increases.
    ![Builder Complex Table Definition](https://raw.githubusercontent.com/Oyelowo/rust-talk/a9b986e5b8dad179d8968dd461e088ba87f8e142/RUST_TALK/builder_complex_table_definition.png)
    ![Create Complex Queries](https://raw.githubusercontent.com/Oyelowo/rust-talk/a9b986e5b8dad179d8968dd461e088ba87f8e142/RUST_TALK/create_complex.png)
    ![Subquery and Conditional Query](https://raw.githubusercontent.com/Oyelowo/rust-talk/a9b986e5b8dad179d8968dd461e088ba87f8e142/RUST_TALK/subquery_and_conditional_query.png)

Code snippets for CRUD operations demonstrate the ORM’s capabilities:
```rust
// Insert operation
use surreal_orm::statements::insert;
let spaceships = vec![
    SpaceShip {
        id: SpaceShip::create_simple_id(),
        name: "Millennium Falcon".to_string(),
        age: 79,
    },
    SpaceShip {
        name: "Starship Enterprise".to_string(),
        age: 15,
        ..Default::default()
    },
];
insert(spaceships).return_many(db.clone()).await?;

// Update operation
use surreal_orm::statements::update;
let space_ship = SpaceShip::table_name();
update::<SpaceShip>(space_ship)
    .content(SpaceShip {
        name: "Oyelowo".to_string(),
        age: 90,
        ..Default::default()
    })
    .where_(cond(strength.greater_than(5)).and(strength.less_than_or_equal(15)))
    .return_many(db.clone())
    .await?;

// Delete operation
use surreal_orm::{*, statements::{delete}};
let space_ship::Schema { name, age, .. } = SpaceShip::schema();
let space_ship = SpaceShip::table_name();
delete(space_ship)
    .where_(cond(name.equal("Millennium Falcon")).and(age.less_then(50)))
    .run(db.clone())
    .await?;
```

- **Query Turbo**: A novel feature that simplifies crafting complex transactions and queries in idiomatic Rust.
    ![Complex Transactions](https://raw.githubusercontent.com/Oyelowo/rust-talk/a9b986e5b8dad179d8968dd461e088ba87f8e142/RUST_TALK/complex_transaction_as_rust.png)

Here's an example of a more intricate query leveraging Rust’s idiomatic structures:
```rust
let account = Account::table();
let id1 = &Account::create_id("one".to_string());
let id2 = &Account::create_id("two".to_string());
let amount_to_transfer = 300.00;
let acc = Account::schema();
let account::Schema { balance, .. } = Account::schema();
let query_chain = query_turbo! {
    // Complex transaction logic here
};
```

- **Automatic Inference of Database Fields' Types and Table Definitions**: Automatically determines the types of database fields from Rust data structures.
    ![Table Structure Definition](https://raw.githubusercontent.com/Oyelowo/rust-talk/a9b986e5b8dad179d8968dd461e088ba87f8e142/RUST_TALK/table_struct_def_with_auto_inferred_db_types.png)

- **Type-Safe IDs**: Guarantees type safety throughout your application by supporting both simple and complex identifiers.
    ![Typed IDs](https://raw.githubusercontent.com/Oyelowo/rust-talk/a9b986e5b8dad179d8968dd461e088ba87f8e142/RUST_TALK/typed_id.png)

- **Automated Schema Migration

**: Facilitates fully automated schema migrations that require no active database connection.
    ![Embedded Migrations](https://raw.githubusercontent.com/Oyelowo/rust-talk/a9b986e5b8dad179d8968dd461e088ba87f8e142/RUST_TALK/embedded_migrations.png)

- **Cross-Validation of DB Field Types with Rust Types**: Ensures consistency between your database and your Rust code.
    ![Field Type Inference Issues](https://raw.githubusercontent.com/Oyelowo/rust-talk/a9b986e5b8dad179d8968dd461e088ba87f8e142/RUST_TALK/unable_to_infer_field_type.png)

- **ORM Capabilities**: Provides a comprehensive set of ORM features to manage database entities more effectively.
    ![Create Spaceship](https://raw.githubusercontent.com/Oyelowo/rust-talk/a9b986e5b8dad179d8968dd461e088ba87f8e142/RUST_TALK/create_spaceship.png)

### Future and Vision for Surreal ORM

While my journey with Surreal ORM began with SurrealDB, the future holds expansions to support popular databases like PostgreSQL, MySQL, SQLite, MongoDB, and Cassandra. This vision includes enhancing Rust's capabilities in database management, pushing the boundaries further in both efficiency and productivity. This project started as a quest to fill a void in the Rust ecosystem, and it remains committed to evolving and meeting the community's needs.