### Description

## The problems


all images names. Paths here are invalid:
/Users/oyelowo/Desktop/RUST_TALK/create_complex.png /Users/oyelowo/Desktop/RUST_TALK/many_to_many_relaton.png /Users/oyelowo/Desktop/RUST_TALK/create_complex2.png /Users/oyelowo/Desktop/RUST_TALK/many_to_many_relation_checked.png /Users/oyelowo/Desktop/RUST_TALK/subquery_and_conditional_query.png /Users/oyelowo/Desktop/RUST_TALK/helper_crud_methods.png /Users/oyelowo/Desktop/RUST_TALK/builder_complex_table_definition.png /Users/oyelowo/Desktop/RUST_TALK/create_with_loaders.png /Users/oyelowo/Desktop/RUST_TALK/helper_crud_method2.png /Users/oyelowo/Desktop/RUST_TALK/embedded_migrations.png /Users/oyelowo/Desktop/RUST_TALK/deep_graph_field_traversal_with_queries.png /Users/oyelowo/Desktop/RUST_TALK/table_struct_def_with_auto_inferred_db_types.png /Users/oyelowo/Desktop/RUST_TALK/Create_statement.png /Users/oyelowo/Desktop/RUST_TALK/deep_graph_field_traversal_with_self_references_field.png /Users/oyelowo/Desktop/RUST_TALK/statically_checked_field_assignment.png /Users/oyelowo/Desktop/RUST_TALK/create_statement_and_loaders.png /Users/oyelowo/Desktop/RUST_TALK/create_spaceship.png /Users/oyelowo/Desktop/RUST_TALK/helper_functions_find_where_is_null.png


Focus your attention on building software rather than ensuring your query correctness

Problems:
DB toolings either don't offer sufficient capabilities for complex tax or when they do,
they tend to be very difficult to work with, write and read.
…What did i do to solve this? Eat your cake and have it! Typed raw queries supported and even 
when writing in rust, it should feel and read just like the Raw query read from left to right

Instant productivity. Get productive within 5 minutes of getting to know surreal-orm/yequery


Unreadable syntax

Friction: Requiring live database to check raw queries or run migrations

Requiring lots of manual work to create, sync and update migration files

Lack of support for embedded migrations

Fragmentation, support some but not other capabilities

Siloed approach. A holistic tightly integrated approach necessary

Either leave too much work for Devs to get basic things working well and intuitive
or overly abstract in a way that it becomes difficult to map mentally to what is happening
behind the scene

Full compliance. Support for all functions, params, relations, querires etc



Solution:
Intuitive API even for very complex queries
Typed Simple and complex IDs. You can use associated method on a struct to create an id which can and only will be assigned to the struct id but invalid when used on other structs.

Most databast type can be autoinferred, so you dont need to
explicitly specify with the surreal_orm field macro ty attribute, and even when you specify, it is cross checked at compile time against your rust field type
Statically checked raw queries and parametization
Support for complex query building


Query Turbo: Query Turbo is a novel concept introduced that enables Complex Raw queries and transaction as rust.
This supports complex queries with support for rust-like 
nested for loop, nested if else statements, statemetns,
expressions, return statement and more for writing extremely complex yet idiomatic and simply written queries

Fully automated schema migration(One-way and two-way)
ORM capabilities
Planned support for other RDBMS(Postgres, MySQL, SQLlite, Mongo, Cassandra etc)
Automated parametization of expressions
Deep typed graph traversal
Compiled time Partial object value for update query.
Object partial with support for strict value assignment, parameters, fields and subqueries
Loaders for automatically loading foreign objects dynamically




complex_transaction_as_rust.png
field_type_check_planet_score.png
nested_field_type_check.png
nested_field_type_check2.png
object_partial_complex_math_expression_block.png
partial_builder_with_null_serialization.png
static_check_raw_query.png
static_check_raw_query_single.png
static_check_raw_query_var.png
static_check_single_raw_query.png
statically_checked_raw_multiple_queries.png
statically_checked_raw_query2.png
table_name_check.png
typed_id.png
unable_to_infer2.png
unable_to_infer_field_type.png


**Surreal ORM: Simplifying Rust Database Interactions**

Discover Surreal ORM, designed to fill the gap in existing database tools for Rust. This project, initially started as a weekend proof of concept, has evolved over 17 months into a robust tool emphasizing type safety and ease of use. Highlights include:

- **Type Safety and Intuitive Use**: Ensures that database operations are both safe and straightforward, integrating static type checks directly into the query building process to prevent common errors before runtime.

    ![Type Check in Action](https://raw.githubusercontent.com/Oyelowo/rust-talk/a9b986e5b8dad179d8968dd461e088ba87f8e142/RUST_TALK/static_check_raw_query.png)

- **Complex Transactions as Native Rust Code**: Manages complex transactions seamlessly using Rust's powerful syntax.

    ![Complex Transaction Example](https://raw.githubusercontent.com/Oyelowo/rust-talk/a9b986e5b8dad179d8968dd461e088ba87f8e142/RUST_TALK/complex_transaction_as_rust.png)

- **Advanced Query Capabilities**: Supports sophisticated queries including nested fields and complex mathematical expressions without sacrificing readability or performance.

    ![Nested Field Query](https://raw.githubusercontent.com/Oyelowo/rust-talk/a9b986e5b8dad179d8968dd461e088ba87f8e142/RUST_TALK/nested_field_type_check.png)
    ![Math Expression in Query](https://raw.githubusercontent.com/Oyelowo/rust-talk/a9b986e5b8dad179d8968dd461e088ba87f8e142/RUST_TALK/object_partial_complex_math_expression_block.png)

- **Static Query Checking**: Each query is checked at compile-time, drastically reducing runtime errors and enhancing code quality.

    ![Static Check on Single Query](https://raw.githubusercontent.com/Oyelowo/rust-talk/a9b986e5b8dad179d8968dd461e088ba87f8e142/RUST_TALK/static_check_single_raw_query.png)
    ![Multiple Query Checks](https://raw.githubusercontent.com/Oyelowo/rust-talk/a9b986e5b8dad179d8968dd461e088ba87f8e142/RUST_TALK/statically_checked_raw_multiple_queries.png)

- **Ease of Setup and Use**: Quick setup and minimal configuration required to get started.

    ![Setup and Initialization](https://raw.githubusercontent.com/Oyelowo/rust-talk/a9b986e5b8dad179d8968dd461e088ba87f8e142/RUST_TALK/Screenshot_2024-04-18_at_11.02.11.png)

Join us to explore how Surreal ORM can streamline your Rust database projects, making them simpler and more error-free.


Long term vision

Planned support for other RDBMS(Postgres, MySQL, SQLlite, Mongo, Cassandra etc)
