What is this?
-------------

A single T4 file that mocks your Linq to SQL data context and lets you write tests against an in-memory version of it.

Setup
--------

1. Copy Rename.extensions.tt into your project, in the same folder as your DBML
2. Rename Rename.extensions.tt to [YourDBMLName].extensions.tt


How to use it
-------------

Say your data context is called `ExampleDataContext`. The mock data context will be called
`MemoryExampleDataContext`. You can use it just like you would your normal data context—it will
have all the same tables. Everything is stored in memory, so it's very fast for testing.

    using(var db = new MemoryExampleDataContext()) {
      db.Products.InsertOnSubmit(new Product { SKU = "example" });
      db.SubmitChanges();
      
      Console.WriteLine(db.Products.First().SKU);
      // prints "example"
    }

The interface that makes this all testable is called `I[YourDBMLName]DataContext`. If you have
code that's using a concrete type of `ExampleDataContext`...

    public static List<Product> GetProducts(ExampleDataContext db) {
      return db.Products().ToList();
    }
    
... you can make it testable by using the interface `IExampleDataContext`.

    public static List<Product> GetProducts(IExampleDataContext db) {
      return db.Products().ToList();
    }

That way, you can call GetProducts with a `MemoryExampleDataContext` for testing, and a real
`ExampleDataContext` in production.


