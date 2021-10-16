1- Connection 
	
	"PostgreSQL": {
		"ConnectionString": "server=localhost;port=5432;userid=postgres;database=joincoder;",
		"DbPassword": "love"
	}

		var connectionString = Configuration["PostgreSql:ConnectionString"];
		var dbPassword = Configuration["PostgreSql:DbPassword"];

		var builder = new NpgsqlConnectionStringBuilder(connectionString)
		{
			Password = dbPassword
		};

		services.AddDbContext<ApplicationContext>(options => options.UseNpgsql(builder.ConnectionString));
		
		
			
2- ApplicationContext 

    public class ApplicationContext: DbContext
    {
        public ApplicationContext(DbContextOptions options): base(options)
        {
        }

        public DbSet<Student> students { get; set; }
    }

3- Model 
	public class Student
	{
		public string id { get; set; }
		public string name { get; set; }
		public string age { get; set; }
	}

	
4- Controller 

        private readonly ApplicationContext _context;
        public StudentController(ApplicationContext context)
        {
            _context = context;
        }
        public IActionResult studentlist()
        {
            ViewBag.users = _context.students.ToList();
            return View();
        }
5- View 

	@{
		ViewData["Title"] = "Student list";
	}
	<h1>@ViewData["Title"]</h1>

	<p>Student list.</p>

	<table class="table table-striped table-bordered dt-responsive nowrap table-sbm" style="border-collapse: collapse; border-spacing: 0; width: 100%;">
		<thead>
			<tr>
				<th>Student ID</th>
				<th>Student Name</th>
				<th>Student Age</th>
			</tr>
		</thead>

		<tbody>
			@{
				foreach (var a in (List<JoinCoder.Models.Student>)ViewBag.users)
				{
					<tr>
						<td>@a.id</td>
						<td>@a.name</td>
						<td>@a.age</td>
					</tr>
				}
			}
		</tbody>

		</table>
6- _layout

		<li class="nav-item">
			<a class="nav-link text-dark" asp-area="" asp-controller="Student" asp-action="studentlist">Student list</a>
		</li>