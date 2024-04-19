### Solutions and Features

Surreal ORM was crafted to revolutionize working with databases in Rust. As a solo developer, over 17 intense months beginning December 2022, I developed this tool from a personal frustration with existing solutions. My goal was to dramatically enhance developer productivity and minimize bugs by offering robust, innovative tools. Here's what sets Surreal ORM apart:

- **Statically Checked Raw Queries with no need for a database connection**: All queries are checked at compile time, minimizing runtime errors and enhancing code reliability.
    ![Statically Checked Raw Queries](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/static_check_raw_query.png)
    ![Multiple Queries Checked](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/statically_checked_raw_multiple_queries.png)

- **Intuitive API for Complex Queries**: The API is designed to simplify your database interactions, even as query complexity increases.
 **Select Query**
    ![Subquery and Conditional Query](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/subquery_and_conditional_query_new.png)

 **Complex Table Definition**
    ![Builder Complex Table Definition](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/builder_complex_table_definition.png)

  <!--   ![Create Complex Queries](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/create_complex.png) -->

Code snippets for CRUD operations demonstrate the ORM’s capabilities:
 **Insert operation**
 ![Insert Query](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/insert.png)

 **Update operation**
 ![Update Query](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/update.png)

 **Delete operation**
 ![Delete Query](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/delete.png)


- **Query Turbo**: A novel feature that simplifies writing complex transactions and queries in idiomatic Rust-like looking syntax with support for nested for loops, nested if-else, let, and return statements.
    ![Complex Transactions](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/complex_transaction_as_rust.png)

Here's an example of a more intricate query leveraging Rust’s idiomatic structures:
```rust
    let account = Account::table():
    let id1 = &Account::create_id("one".to_string());
    let id2 = &Account::create_id("two".to_string());
    let amount_to_transfer = 300.00;

    let acc = Account::schema();
    let account::Schema { balance, .. } = Account::schema();

    let query_chain = query_turbo! {
        BEGIN TRANSACTION;
         let balance1 = create_only().content(Balance {
                id: Balance::create_id("balance1".to_string()),
                amount: amount_to_transfer,
            });

         create_only().content(Balance {
                id: Balance::create_id("balance2".to_string()),
                amount: amount_to_transfer,
            });

        if balance.greater_than(100) {
            let first_name = "Mars";
            let score = 100;
            select(All).from(account).where_(acc.balance.eq(5));
        } else if balance.less_than(100) {
            let first_name = "Jupiter";
            let score = 100;
            select(All).from(account).where_(acc.balance.eq(5));
        }  else {
            let first_name = "Earth";
            let score = 100;
            select(All).from(account).where_(acc.balance.eq(5));
        };

        for name in vec!["Jupiter", "Alberta"] {
            let first = "Vancouver";
            select(All).from(account).where_(acc.balance.eq(5));

            let good_stmt = select(All).from(account).where_(acc.balance.eq(64));

            if balance.gt(50) {
                let first_name = "Pluto";
            };

            select(All).from(account).where_(acc.balance.eq(34));

            let numbers = vec![23, 98];

            for age in numbers {
              let score = 100;
              let first_stmt = select(All).from(account).where_(acc.balance.eq(5));

              let second_stmt = select(All).from(account).where_(acc.balance.eq(25));
              select(All).from(account).where_(acc.balance.eq(923));

            };
        };

         let  balance3 = create().content(Balance {
                id: Balance::create_id("balance3".into()),
                amount: amount_to_transfer,
            });

        let accounts = select(All)
            .from(id1..=id2);


        // You can reference the balance object by using the $balance variable and pass the amount
        // as a parameter to the decrement_by function. i.e $balance.amount
        let updated1 = update::<Account>(id1).set(acc.balance.increment_by(balance1.with_path::<Balance>(E).amount));
        update::<Account>(id1).set(acc.balance.increment_by(balance1.with_path::<Balance>(E).amount));
        update::<Account>(id1).set(acc.balance.increment_by(45.3));

        // You can also pass the amount directly to the decrement_by function. i.e 300.00
        update::<Account>(id2).set(acc.balance.decrement_by(amount_to_transfer));
        update::<Account>(id2).set(acc.balance.decrement_by(50));

        COMMIT TRANSACTION;
    };

    let _result = query_chain.run(db.clone()).await?;

    let accounts = select(All)
        .from(id1..=id2)
        .return_many::<Account>(db.clone())
        .await?;

```


- **Automatic Inference of Database Fields' Types and Table Definitions**: Saves developer time by automatically inferring the types of database fields from Rust data structures and cross-validating to prevent false positives/negatives. Field definitions for user can be extracted using `User::define_fields()`
    ![Table Structure Definition](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/table_struct_def_with_auto_inferred_db_types.png)

- **Type-Safe IDs**: Enhances application safety by ensuring that identifiers are correct across different contexts, preventing common bugs at compile time.
    ![Typed IDs](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/typed_id.png)

- **Automated Schema Migration**: Offers fully automated schema migration with no need for external database running, both one-way and two-way, reducing manual intervention. With auto detection of migration type(one-way or two-way) after initialization
  
  Example usage:

  Gathering resources from codebase and struct definitions
```rust
use surreal_orm::*;

#[derive(Node, Serialize, Deserialize, Debug, Clone, Default)]
#[serde(rename_all = "camelCase")]
#[surreal_orm(table_name = "animal", schemafull)]
pub struct Animal {
    pub id: SurrealSimpleId<Self>,
    pub species: String,
    // #[surreal_orm(old_name = "oldField")] // Comment this line out to carry out a renaming operation
    pub attributes: Vec<String>,
    pub created_at: chrono::DateTime<Utc>,
    pub updated_at: chrono::DateTime<Utc>,
    pub velocity: u64,
}

impl TableResources for Animal {
    fn events_definitions() -> Vec<Raw> {
        let animal::Schema { species, velocity, .. } = Self::schema();

        let event1 = define_event("event1".to_string())
            .on_table("animal".to_string())
            .when(cond(species.eq("Homo Erectus")).and(velocity.gt(545)))
            .then(select(All).from(Crop::table_name()))
            .to_raw();

        vec![event1]
    }

    fn indexes_definitions() -> Vec<Raw> {
        let animal::Schema { species, velocity, .. } = Self::schema();

        let idx1 = define_index("species_speed_idx".to_string())
            .on_table(Self::table_name())
            .fields(arr![species, velocity])
            .unique()
            .to_raw();

        vec![idx1]
    }
}

#[derive(Edge, Serialize, Deserialize, Debug, Clone, Default)]
#[serde(rename_all = "camelCase")]
#[surreal_orm(table_name = "eats", schemafull)]
pub struct Eats<In: Node, Out: Node> {
    pub id: SurrealSimpleId<Self>,
    #[serde(rename = "in")]
    pub in_: In,
    pub out: Out,
    pub place: String,
    pub created_at: chrono::DateTime<Utc>,
}

pub type AnimalEatsCrop = Eats<Animal, Crop>;
impl TableResources for AnimalEatsCrop {}

#[derive(Debug, Clone)]
pub struct Resources;

impl DbResources for Resources {
    create_table_resources!(
        Animal,
        Crop,
        AnimalEatsCrop,
    );

    // Define other database resources here. They default to empty vecs
    fn analyzers(&self) -> Vec<Raw> {
        vec![]
    }

    fn functions(&self) -> Vec<Raw> {
        vec![]
    }

    fn params(&self) -> Vec<Raw> {
        vec![]
    }

    fn scopes(&self) -> Vec<Raw> {
        vec![]
    }

    fn tokens(&self) -> Vec<Raw> {
        vec![]
    }

    fn users(&self) -> Vec<Raw> {
        vec![]
    }
}


use surreal_orm::migrator::Migrator;

#[tokio::main]
async fn main() {
    Migrator::run(Resources).await;
}
```

Example migration commands:
```sh
# Check information about all commands
cargo run -- help

# Check information about specific commands
cargo run -- init --help

cargo run -- init --name "initial_migration" -r

cargo run -- gen --name "add_users_table"

# Applies all pending till latest by default
cargo run -- up

# Applies all pending till latest
cargo run -- up -l

# Applies by the number specified
cargo run -- up -n 5
cargo run -- up --number 5

# Applies till specified migration
cargo run -- up -t "20240107015727114_create_first.up.surql"
cargo run -- up --till "20240107015727114_create_first.up.surql"


# Rollback migration to previous by default
cargo run -- down

# Rollback all pending till previous
cargo run -- down --previous

# Rollback by the number specified
cargo run -- down -n 5
cargo run -- down --number 5

# Rollback till specified migration
cargo run -- down -t "20240107015727114_create_first.up.surql"
cargo run -- down --till "20240107015727114_create_first.up.surql"

# In addition, you can use the --prune flag to delete local migration 
# files after rolling back. This can be useful in development for rapid changes.
cargo run -- down -n 5 --prune

# Rollback migration to previous by default
cargo run -- down

# Rollback all pending till previous
cargo run -- down --previous

# Rollback by the number specified
cargo run -- down -n 5
cargo run -- down --number 5

# Rollback till specified migration
cargo run -- down -t "20240107015727114_create_first.up.surql"
cargo run -- down --till "20240107015727114_create_first.up.surql"

# In addition, you can use the --prune flag to delete local migration 
# files after rolling back. This can be useful in development for rapid changes.
cargo run -- down -n 5 --prune

cargo run -- reset --name "initial_migration" -r

# List pending migrations by default
cargo run -- prune

# List pending migrations by default
cargo run -- ls
cargo run -- list

# List all migrations
cargo run -- list --status all

# List pending migrations
cargo run -- list --status pending

# List applied migrations
cargo run -- list --status applied
```

 **Embedded Migrations**
  ![Embedded Migrations](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/embedded_migrations.png)  

- **Statically checked and cross-validated DB field types with Rust Types**: Ensures consistency between your database and your Rust code.
    ![Field Type Inference Issues](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/unable_to_infer_field_type.png)

    ![field_type_check_planet_score](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/field_type_check_planet_score.png)

    ![nested_field_type_check](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/nested_field_type_check.png)

    ![table_name_check](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/table_name_check.png)

- **ORM Capabilities**: Provides a comprehensive set of ORM features to manage database entities more effectively.
    ![Create Spaceship](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/create_spaceship.png)

    ![Create statement](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/Create_statement.png)

    ![Crud Helper methods](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/helper_crud_methods.png)
    ![More Crud Helper methods](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/helper_crud_method2.png)

    ![Record Filtering](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/helper_functions_find_where_is_null.png)

    ![Many-to-Many Relations](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/many_to_many_relation_checked.png)




- **Automated Expression Parameterization**: Automates the parameterization of expressions, making it easy and readable to write more complex mathematical expressions in queries.
      ![Automated Expression Parameterization](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/object_partial_complex_math_expression_block.png)




- **Deep Typed Graph Traversal**: This feature enables you to navigate and manage complex relationships within your database effortlessly. By utilizing deep graph traversal techniques, you can explore interconnected data with precision and ease, ensuring that every query not only reaches its target efficiently but does so with an understanding of the data's structure.

      ![deep_graph_field_traversal_with_queries](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/deep_graph_field_traversal_with_queries.png)

      ![deep graph field traversal with self references field](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/deep_graph_field_traversal_with_self_references_field.png)


- **Statically checked Partial Object Updates and field assignment**: Supports updates using partial objects, which can be statically checked at compile time, ensuring that only valid data is submitted.
      ![partial_builder_with_null_serialization](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/partial_builder_with_null_serialization.png)

      ![object_partial_complex_math_expression_block](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/object_partial_complex_math_expression_block.png)

      ![statically_checked_field_assignment](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/statically_checked_field_assignment.png)

  

- **Dynamic Loaders**: Simplifies the loading of related foreign objects dynamically, making your data retrieval processes more efficient.
      ![create_statement_and_loaders](https://raw.githubusercontent.com/Oyelowo/rust-talk/master/RUST_TALK/create_statement_and_loaders.png)





### Future and Vision for Surreal ORM
This does not stop here. Although, I started this experiment with SurrealDB - a graph database written in Rust(I have no affiliation with the company nor founders), future updates will include support for popular databases like PostgreSQL, MySQL, SQLite, MongoDB, and Cassandra and adding other features that continue to push the boundaries of what you can do with Rust and databases.

Our mission is to help developers move fast without breaking things and get out the way

