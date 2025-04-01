# Entity Framework Core 

Entity Framework Core is an open-source object-relational mapping (ORM) framework for .NET applications. It provides a set of tools and libraries to interact with databases using .NET objects.
It allows both a code-first and database-first approach, enabling developers to define their data models in code or generate them from an existing database schema.
It supports LINQ queries, change tracking, and migrations, making it easier to manage database schema changes over time.

It contains a number of providers for different databases, including SQL Server, SQLite, PostgreSQL, and MySQL. This allows developers to work with a variety of databases using a consistent API.

It also supports asynchronous programming, enabling developers to build responsive applications that can handle multiple database operations concurrently.

## DBContext

The `DbContext` class is the primary class responsible for interacting with the database in Entity Framework Core. It serves as a bridge between your application and the database, allowing you to perform CRUD (Create, Read, Update, Delete) operations on your data models.
It manages the database connection, tracks changes to entities, and provides methods for querying and saving data.

A core part of the DbContext is the 'OnConfiguring' method, which is used to configure the context options, such as the database provider and connection string. This method is typically overridden in a derived context class to set up the context for a specific database.

An example of a DbContext class might look like this:

```csharp
using Microsoft.EntityFrameworkCore;

public class MyDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }
    public DbSet<Category> Categories { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer("YourConnectionStringHere");
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        // Configure entity relationships and properties here
        modelBuilder.Entity<Product>().HasKey(p => p.Id);
        modelBuilder.Entity<Category>().HasKey(c => c.Id);
    }
}
```
In this example, the `MyDbContext` class inherits from `DbContext` and defines two `DbSet` properties for the `Product` and `Category` entities. The `OnConfiguring` method is overridden to specify the SQL Server database connection string, and the `OnModelCreating` method is used to configure the entity relationships and properties.

The `DbSet` properties represent collections of entities in the database, allowing you to perform LINQ queries and other operations on them. The `OnModelCreating` method is where you can configure entity relationships, constraints, and other model-specific settings using the Fluent API.

Additionally there is the DataAnnotations approach, which allows you to use attributes on your entity classes to configure the model. This is a more declarative way of configuring your entities and can be used in conjunction with the Fluent API.
For example, you can use the `[Key]` attribute to specify the primary key for an entity, or the `[Required]` attribute to specify that a property is required.

```csharp
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

public class Product
{
    [Key]
    public int Id { get; set; }

    [Required]
    public string Name { get; set; }

    [Column(TypeName = "decimal(18,2)")]
    public decimal Price { get; set; }
}
```
In this example, the `Product` class uses the `[Key]` attribute to specify that the `Id` property is the primary key, and the `[Required]` attribute to indicate that the `Name` property is required. The `[Column]` attribute is used to specify the column type for the `Price` property.



## Migrations

Migrations in Entity Framework Core are a way to manage changes to the database schema over time. They allow you to create, update, and delete database tables and columns based on changes made to your data model classes.

When you make changes to your entity classes, such as adding or removing properties, you can create a migration that captures those changes. The migration can then be applied to the database to update its schema accordingly.
Migrations are typically created using the `Add-Migration` command in the Package Manager Console or the `dotnet ef migrations add` command in the command line. Once a migration is created, it can be applied to the database using the `Update-Database` command or `dotnet ef database update` command.

Using EF migrations allows you to version control your database schema changes, making it easier to collaborate with other developers and maintain a consistent database structure across different environments (development, staging, production). Additionally when clients may be on differing versions of the database, migrations can be used to bring them up to date with the latest schema changes even when starting from different versions.

EF uses a datetime stamp to identify the migration, so you can have multiple migrations in a single project. Each migration is represented by a class that inherits from `Migration`, and it contains methods for applying and reverting the changes to the database. These are the 'Up' and 'Down' methods, respectively. The 'Up' method is used to apply the changes, while the 'Down' method is used to revert them.

For example, a migration class might look like this:

```csharp
using Microsoft.EntityFrameworkCore.Migrations;

public partial class AddProductTable : Migration
{
    protected override void Up(MigrationBuilder migrationBuilder)
    {
        migrationBuilder.CreateTable(
            name: "Products",
            columns: table => new
            {
                Id = table.Column<int>(nullable: false)
                    .Annotation("SqlServer:Identity", "1, 1"),
                Name = table.Column<string>(nullable: false),
                Price = table.Column<decimal>(nullable: false)
            },
            constraints: table =>
            {
                table.PrimaryKey("PK_Products", x => x.Id);
            });
    }

    protected override void Down(MigrationBuilder migrationBuilder)
    {
        migrationBuilder.DropTable(name: "Products");
    }
}
```

In this example, the `AddProductTable` migration creates a new table called `Products` with three columns: `Id`, `Name`, and `Price`. The `Up` method defines the changes to be applied to the database, while the `Down` method defines how to revert those changes.

When you run the migration, Entity Framework Core will execute the `Up` method to create the `Products` table in the database. If you need to revert the migration, you can run the `Down` method to drop the table.

Migrations are a powerful feature of Entity Framework Core that helps you manage your database schema changes in a structured and version-controlled manner. They allow you to evolve your database schema alongside your application code, ensuring that your data model remains in sync with the underlying database structure.

## LINQ Queries

LINQ (Language Integrated Query) is a powerful feature of C# that allows you to query collections of data using a syntax that is similar to SQL. Entity Framework Core supports LINQ queries, enabling you to retrieve and manipulate data from the database using strongly typed queries.

LINQ queries can be written using method syntax or query syntax. Method syntax uses extension methods to build the query, while query syntax uses a more SQL-like syntax. Both approaches are equivalent and can be used interchangeably.

LINQ queries are executed against `DbSet` properties in the `DbContext`, allowing you to perform operations such as filtering, sorting, and grouping on the data.

For example, you can use LINQ to retrieve all products from the database that have a price greater than 100:

```csharp
using (var context = new MyDbContext())
{
    var expensiveProducts = context.Products
        .Where(p => p.Price > 100)
        .ToList();
}
```
In this example, the `Where` method is used to filter the products based on their price, and the `ToList` method is called to execute the query and retrieve the results as a list.
