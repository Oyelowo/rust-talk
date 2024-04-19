### Description

**Surreal ORM: Elevating Rust Database Operations**


Surreal ORM started as my personal quest for a better database tool in Rust. After dealing with the limitations of what was available, I spent 17 months building something that I needed: a straightforward, type-safe database library that just works. 

#### Problems
Rust developers often face challenges with database tooling that is either underpowered or overly complex. This leads to:
- **Limited Capabilities**: Many DB tools either lack the features needed for complex tasks or are too cumbersome to use effectively.
- **Unintuitive Syntax**: The available tools often feature unreadable syntax, making them hard to work with and increasing the learning curve.
- **Operational Friction**: Common requirements for a live database connection to validate queries or run migrations add unnecessary complexity.
- **Manual Migration Hassles**: Developers waste significant time manually creating, syncing, and updating migration files, compounded by a lack of embedded migrations support.
- **Tool Fragmentation**: Many solutions support some features but neglect others, leading to a patchwork of tools rather than a seamless experience.
- **Overly Complex or Simplistic**: Tools are often either too abstract, making it hard to understand the underlying processes, or too simplistic, leaving developers to handle too many basic tasks.
- **Instant Productivity Gap**: Despite promises, few tools allow developers to be productive quickly and intuitively.

Each of these issues represents a barrier to efficiency and effectiveness in database management, which Surreal ORM aims to dismantle.



---



---

### Solutions and Features

Surreal ORM was built to make working with databases in Rust not just easier, but also smarter and more efficient. Here’s how:

- **Statically Checked Raw Queries with no need for db connection**: All queries are checked at compile time, minimizing runtime errors and enhancing code reliability.


static_check_raw_query_single.png
statically_checked_raw_multiple_queries.png



- **Intuitive API for Complex Queries**: Simplifies your interactions with databases, even when the queries get complex.
  

builder_complex_table_definition.png
create_complex.png
subquery_and_conditional_query.png

Insert
```rust
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
```

Update
```rust
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
```

Delete
```rust
use surreal_orm::{*, statements::{delete}};

let space_ship::Schema { name, age, .. } = SpaceShip::schema();
let space_ship = SpaceShip::table_name();

delete(space_ship)
    .where_(cond(name.equal("Millennium Falcon")).and(age.less_then(50)))
    .run(db.clone())
    .await?;
```

- **Query Turbo**: Novel idea that simplifies writing of complex queries and transactions using idiomatic Rust, making advanced database operations straightforward.

complex_transaction_as_rust.png

Or even much more complex queries yet with easy-to-read and write idiomatic rust-like statically nested if else, nested for loop statements
```rust
    let account = Account::table():
    let id1 = &Account::create_id("one".to_string());
    let id2 = &Account::create_id("two".to_string());
    let amount_to_transfer = 300.00;

    let acc = Account::schema();
    let account::Schema { balance, .. } = Account::schema();

    let query_chain = query_turbo! {
        begin transaction;
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

        commit transaction;
    };

    insta::assert_display_snapshot!(query_chain.to_raw().build());
    insta::assert_display_snapshot!(query_chain.fine_tune_params());

    let _result = query_chain.run(db.clone()).await?;

    let accounts = select(All)
        .from(id1..=id2)
        .return_many::<Account>(db.clone())
        .await?;

```




- **Automatic inference of database fields' types and table defintions**
  table_struct_def_with_auto_inferred_db_types.png

- **Cross-validation of DB field types with rust type**:
 unable_to_infer_field_type.png  
 unable_to_infer2.png

- **Type-Safe IDs**: Supports both simple and complex identifiers, ensuring type safety across your application.

 
typed_id.png


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

embedded_migrations.png

- **ORM Capabilities**: Provides robust ORM features, enhancing your ability to manage database entities more effectively.

create_spaceship.png
Create_statement.png
helper_crud_methods.png
helper_crud_method2.png
helper_functions_find_where_is_null.png
many_to_many_relation_checked.png

- **Statically checked database field types and table attributes**: Check db field type aligns with rust type with support for complex nested types.
 table_name_check.png
  field_type_check_planet_score.png
  nested_field_type_check.png


- **Automated Expression Parameterization**: Automates the parameterization of expressions, streamlining complex queries.
  object_partial_complex_math_expression_block.png

- **Deep Typed Graph Traversal**: Allows for intricate data relationship management within your database, using deep graph traversal techniques.


deep_graph_field_traversal_with_queries.png
deep_graph_field_traversal_with_self_references_field.png


- **Statically checked Partial Object Updates and field assignment**: Supports updates using partial objects, which can be statically checked at compile time, ensuring that only valid data is submitted.
 partial_builder_with_null_serialization.png
   object_partial_complex_math_expression_block.png

   statically_checked_field_assignment.png

  

- **Dynamic Loaders**: Simplifies the loading of related objects dynamically, making your data retrieval processes more efficient.
create_statement_and_loaders.png




### Future and Vision for Surreal Orm
We're not stopping here. Although, I started this experiment started with Surrealdb - a graph database written in Rust(Disclaimer: I have no affiliation with the company nor founders), future updates will include support for popular databases like PostgreSQL, MySQL, SQLite, MongoDB, and Cassandra and adding other features that continue to push the boundaries of what you can do with Rust and databases.








