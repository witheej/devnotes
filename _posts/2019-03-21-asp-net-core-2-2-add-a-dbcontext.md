---
layout: post
title: ASP.NET Core 2.x - Add a DbContext
sub_heading: Add an Entity Framework Core DbContext to an existing ASP.NET Core 2.x
  project
date: 2019-03-21 00:00:00 -0400
tags:
- Entity Framework Core
- ASP.NET Core 2.2
- DbContext
banner_image: ''
related_posts: []

---
Index:

1. Example entity model
2. Install NuGet packages
3. Create context class
4. Update Startup.cs
5. Example connection string
6. Add-Migration
7. Update-Database
8. Seed data

Example entity model

    using System.ComponentModel.DataAnnotations;
    
    namespace MyProject.Models
    {
        public class Customer
        {
            [Key]
            public long CustomerId { get; set; }
            public string Name { get; set; }
            public DateTime? FirstPurchaseDate { get; set; }
        }
    }

Read about the `[Key]` annotation and others [here](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations?view=netcore-2.2)

Install NuGet packages (the Identity packages are only necessary if your app is managing identities, I think)

    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.AspNetCore.Identity" Version="2.2.0" />
    <PackageReference Include="Microsoft.AspNetCore.Identity.EntityFrameworkCore" Version="2.2.0" />
    <PackageReference Include="Microsoft.EntityFrameworkCore" Version="2.2.0" />

Create your context class that inherits from [DbContext](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext?view=efcore-2.1).  I like to put my context class in a folder named "Data" at the root of my project.

    using Microsoft.EntityFrameworkCore;
    using MyProject.Models;
    
    namespace MyProject.Data
    {
        public class ApplicationDbContext : DbContext
        {
            public MyProjectDbContext(DbContextOptions<MyProjectDbContext> options) : base(options)
            {
    
            }
    
            // Add DbSet for each entity class
            public DbSet<Customer> Customer { get; set; }
        }
    }

Call `services.AddDbContext<>(...);` in Startup.cs

    using Microsoft.EntityFrameworkCore;
    using Microsoft.Extensions.Configuration;
    using System;
    using MyProject.Data;
    
    namespace MyProject
    {
        public class Startup
        {
            public Startup(IConfiguration configuration)
            {
                Configuration = configuration;
            }
    
            public IConfiguration Configuration { get; }
    
            public void ConfigureServices(IServiceCollection services)
            {
                // ...
    
                services.AddDbContext<MyProjectDbContext>(options => options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));
    
                // ...
            }
            
            public void Configure(IApplicationBuilder app, IHostingEnvironment env, ...)
            {
                // ...
            }
        
        }
    }

Example appsettings.json connection string (the database it points to doesn't need to exist, but the connection string needs to exist for `Add-Migration` to work )

    "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\MSSQLLocalDB;Database=_CHANGE_ME;Trusted_Connection=True;MultipleActiveResultSets=true"
     }

TODO: Add-Migration

TODO: Update-Database

TODO: Initialization code, like seed data 