# crud app with sql server 
# steps
1. After installing the sql server you need to start your server using the following command. 

```bash
sqlcmd -S localhost -U SA -P 'YourPassword'

```

2. Create your App with the following command

```bash
dotnet new mvc -n CrudApp
cd CrudApp

```

3. Configure the Database Connection in .NET 8.

```bash
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Design

```

4.  Create the Model 
```bash
using System.ComponentModel.DataAnnotations;

namespace CrudApp.Models
{
    public class Note
    {
        public int Id { get; set; }

        [Required]
        [StringLength(100)]
        public string Title { get; set; }

        [Required]
        public string Content { get; set; }

        public DateTime CreatedAt { get; set; } = DateTime.Now;
    }
}

```


5. Create a folder Data in the project root and add a new file called AppDbContext.cs.

```bash
using Microsoft.EntityFrameworkCore;
using CrudApp.Models;

namespace CrudApp.Data
{
    public class AppDbContext : DbContext
    {
        public AppDbContext(DbContextOptions<AppDbContext> options) : base(options) { }

        public DbSet<Note> Notes { get; set; }
    }
}

```

6. Update appsettings.json with the database connection string.

```bash
"ConnectionStrings": {
     "DefaultConnection": "Server=localhost,1433;Database=CrudAppDB;User Id=yourUserName;Password=YourPassword;TrustServerCertificate=True;"

}

```

7. Register AppDbContext in Program.cs.

```bash
var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
builder.Services.AddControllersWithViews();
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

var app = builder.Build();

```

8. Generate Migrations and Apply the Database Schema:
```bash
dotnet ef migrations add InitialCreate
dotnet ef database update

```