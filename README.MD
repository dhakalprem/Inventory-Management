Inventory Management System using C#, ASP.NET Core MVC, and Entity Framework 📦

An Inventory Management System (IMS) is a web application used to manage products, stock levels, suppliers, and sales in a business.

This project is built using:

C# – application logic

ASP.NET Core MVC – web application architecture

Entity Framework Core – database operations

SQL Server – store data

It helps businesses track products, stock, and inventory transactions.

1️⃣ What an Inventory Management System Does

Main purpose:

Track available products

Manage stock quantity

Record purchases and sales

Manage suppliers

Generate reports

Example:

Product: Laptop
Stock: 25
Price: ₹70,000
Supplier: Dell
2️⃣ Core Modules of the System
1. Product Management

Admin can manage products.

Features:

Add product

Edit product

Delete product

View product list

Example fields:

Product Name
Category
Price
Stock Quantity
2. Category Management

Products are grouped by categories.

Example:

Electronics
Furniture
Stationery
Clothing
3. Supplier Management

Stores supplier details.

Example:

Supplier Name
Phone
Email
Address
4. Stock Management

Tracks stock quantity.

Example:

Product: Keyboard
Current Stock: 120
Minimum Stock: 20
5. Sales Management

Records product sales.

Example:

Invoice No: 1001
Product: Laptop
Quantity: 2
Total: ₹140000
6. Reports

System can generate reports like:

Product report

Sales report

Low stock report

Purchase report

3️⃣ System Architecture

ASP.NET Core MVC uses three layers.

User
  ↓
Controller
  ↓
Model
  ↓
Database
MVC Structure
Layer	Purpose
Model	Data structure
View	UI (HTML pages)
Controller	Handles requests
4️⃣ Project Folder Structure

Example project structure:

InventoryManagementSystem
│
├── Controllers
│   ├── ProductController.cs
│   ├── CategoryController.cs
│
├── Models
│   ├── Product.cs
│   ├── Category.cs
│   └── Supplier.cs
│
├── Views
│   ├── Product
│   │   ├── Index.cshtml
│   │   ├── Create.cshtml
│   │   └── Edit.cshtml
│
├── Data
│   └── AppDbContext.cs
│
├── wwwroot
│
└── Program.cs
5️⃣ Example Product Model
public class Product
{
    public int Id { get; set; }

    public string Name { get; set; }

    public string Category { get; set; }

    public decimal Price { get; set; }

    public int Quantity { get; set; }
}

This model represents the Product table in the database.

6️⃣ Entity Framework Database Context
public class AppDbContext : DbContext
{
    public AppDbContext(DbContextOptions<AppDbContext> options)
        : base(options)
    {
    }

    public DbSet<Product> Products { get; set; }
}

This connects the application to the database.

7️⃣ Example Product Controller

Controller manages product operations.

public class ProductController : Controller
{
    private readonly AppDbContext _context;

    public ProductController(AppDbContext context)
    {
        _context = context;
    }

    public IActionResult Index()
    {
        var products = _context.Products.ToList();
        return View(products);
    }
}
8️⃣ Example View (Product List)
Product List

ID   Name      Price   Stock
--------------------------------
1    Laptop    70000   10
2    Mouse     500     50

Views are created using Razor (.cshtml).

9️⃣ CRUD Operations

The system supports CRUD operations.

Operation	Description
Create	Add product
Read	View products
Update	Edit product
Delete	Remove product
🔟 Workflow of the System
User opens website
        ↓
Product Controller receives request
        ↓
Entity Framework fetches data
        ↓
SQL Server returns product data
        ↓
View displays products
1️⃣1️⃣ Advantages

✔ Easy inventory tracking
✔ Prevents stock shortages
✔ Automates reports
✔ Reduces manual errors
✔ Centralized data management

1️⃣2️⃣ Real-World Use Cases

Inventory systems are used in:

Retail stores

Warehouses

Supermarkets

Manufacturing companies

Online stores


An Inventory Management System using C#, ASP.NET Core MVC, and Entity Framework is a web application that helps businesses manage products, stock, suppliers, and sales efficiently.


Inventory Management System using:

C#

ASP.NET Core MVC

Entity Framework Core

SQL Server

Visual Studio Code

This guide includes:

1️⃣ Project creation
2️⃣ Database design (ER diagram)
3️⃣ Models
4️⃣ Entity Framework setup
5️⃣ CRUD operations
6️⃣ Build and run commands

1️⃣ Install Required Tools

Install:

.NET SDK 8

SQL Server

VS Code

C# extension

Check installation:

dotnet --version
2️⃣ Create ASP.NET Core MVC Project

Open terminal.

Create project:

dotnet new mvc -n InventoryManagementSystem

Go inside project:

cd InventoryManagementSystem

Open project in VS Code:

code .
3️⃣ Install Entity Framework Packages

Run:

dotnet add package Microsoft.EntityFrameworkCore.SqlServer

dotnet add package Microsoft.EntityFrameworkCore.Tools

Build project:

dotnet build

4️⃣ Project Folder Structure
InventoryManagementSystem
│
├── Controllers
│
├── Models
│
├── Views
│
├── Data
│
├── wwwroot
│
├── appsettings.json
└── Program.cs
5️⃣ Database Design (ER Diagram)

Main tables:

Category
---------
CategoryId
Name

Supplier
---------
SupplierId
Name
Phone
Email

Product
---------
ProductId
Name
Price
Quantity
CategoryId
SupplierId
ER Diagram
Category
   |
   | (1)
   |
   |----< Product >----|
                       |
                       | (1)
                    Supplier

Meaning:

One category → many products

One supplier → many products

6️⃣ Create Models

Create Models/Product.cs

using System.ComponentModel.DataAnnotations;

public class Product
{
    public int ProductId { get; set; }

    [Required]
    public string Name { get; set; }

    public decimal Price { get; set; }

    public int Quantity { get; set; }

    public int CategoryId { get; set; }

    public int SupplierId { get; set; }
}

Create Models/Category.cs

public class Category
{
    public int CategoryId { get; set; }

    public string Name { get; set; }
}

Create Models/Supplier.cs

public class Supplier
{
    public int SupplierId { get; set; }

    public string Name { get; set; }

    public string Phone { get; set; }

    public string Email { get; set; }
}
7️⃣ Create Database Context

Create folder Data

Create file Data/AppDbContext.cs

using Microsoft.EntityFrameworkCore;
using InventoryManagementSystem.Models;

public class AppDbContext : DbContext
{
    public AppDbContext(DbContextOptions<AppDbContext> options)
        : base(options)
    {
    }

    public DbSet<Product> Products { get; set; }

    public DbSet<Category> Categories { get; set; }

    public DbSet<Supplier> Suppliers { get; set; }
}
8️⃣ Configure Database Connection

Open appsettings.json

Add connection string:

"ConnectionStrings": {
"DefaultConnection": "Server=localhost;Database=InventoryDB;Trusted_Connection=True;TrustServerCertificate=True"
}
9️⃣ Register DbContext

Open Program.cs

Add:

builder.Services.AddDbContext<AppDbContext>(options =>
options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

Add using:

using Microsoft.EntityFrameworkCore;
🔟 Create Database Migration

Run:

dotnet ef migrations add InitialCreate

Apply migration:

dotnet ef database update

Now database InventoryDB is created.

1️⃣1️⃣ Create Product Controller

Create Controllers/ProductController.cs

using Microsoft.AspNetCore.Mvc;
using InventoryManagementSystem.Models;

public class ProductController : Controller
{
    private readonly AppDbContext _context;

    public ProductController(AppDbContext context)
    {
        _context = context;
    }

    public IActionResult Index()
    {
        var products = _context.Products.ToList();
        return View(products);
    }

    public IActionResult Create()
    {
        return View();
    }

    [HttpPost]
    public IActionResult Create(Product product)
    {
        _context.Products.Add(product);
        _context.SaveChanges();
        return RedirectToAction("Index");
    }

    public IActionResult Delete(int id)
    {
        var product = _context.Products.Find(id);

        _context.Products.Remove(product);

        _context.SaveChanges();

        return RedirectToAction("Index");
    }
}
1️⃣2️⃣ Create Views

Create folder:

Views/Product
Index.cshtml
@model IEnumerable<Product>

<h2>Products</h2>

<a href="/Product/Create">Add Product</a>

<table border="1">
<tr>
<th>ID</th>
<th>Name</th>
<th>Price</th>
<th>Quantity</th>
<th>Action</th>
</tr>

@foreach(var item in Model)
{
<tr>
<td>@item.ProductId</td>
<td>@item.Name</td>
<td>@item.Price</td>
<td>@item.Quantity</td>
<td>
<a href="/Product/Delete/@item.ProductId">Delete</a>
</td>
</tr>
}
</table>
Create.cshtml
@model Product

<h2>Add Product</h2>

<form method="post">

Name:
<input asp-for="Name" />

Price:
<input asp-for="Price" />

Quantity:
<input asp-for="Quantity" />

<button type="submit">Save</button>

</form>
1️⃣3️⃣ Run the Project

Run command:

dotnet run

Open browser:

https://localhost:5001/Product

You can now:

✔ Add product
✔ View products
✔ Delete product

1️⃣4️⃣ Build Project
dotnet build
1️⃣5️⃣ Publish Project
dotnet publish -c Release

Output folder:

bin/Release/net8.0/publish
1️⃣6️⃣ Inventory System Features

This project now supports:

✔ Product CRUD
✔ Category management
✔ Supplier management
✔ Database integration
✔ MVC architecture

1️⃣7️⃣ Real Production Features (Advanced)

Real systems also include:

Login system

Role-based admin

Sales and purchase module

Stock alerts

Dashboard reports

Barcode scanning

✅ Now you have a complete Inventory Management System project structure with CRUD using ASP.NET Core MVC and Entity Framework.