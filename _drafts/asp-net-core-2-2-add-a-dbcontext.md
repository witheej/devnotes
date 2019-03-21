---
layout: post
title: ASP.NET Core 2.2 - Add a DbContext
sub_heading: ''
date: 2019-03-21 00:00:00 -0400
tags:
- Entity Framework Core
- ASP.NET Core 2.2
- DbContext
banner_image: ''
related_posts: []

---
Tasks:

1. Create entity classes
2. Install NuGet packages
3. Create DbContext class
4. 
5. Initializer
6. Connection string
7. Startup.cs
8. Code-First Migration

I like to put this in a folder named "Data" at the root of my project.

Install NuGet packages

    <PackageReference Include="EntityFramework" Version="6.2.0" />
    <PackageReference Include="Microsoft.AspNet.Identity.EntityFramework" Version="2.2.2" />

Create DbContext class

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

Update Startup.cs

    using Microsoft.EntityFrameworkCore;
    using Microsoft.Extensions.Configuration;
    using System;
    using MyProject.Data;
    
    namespace UserManagementPortal
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