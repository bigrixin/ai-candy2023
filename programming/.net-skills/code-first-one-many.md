# Code first one-many

[https://learn.microsoft.com/en-us/ef/core/modeling/relationships/one-to-many](https://learn.microsoft.com/en-us/ef/core/modeling/relationships/one-to-many)

{% code fullWidth="true" %}
```
// Create class
User.cs
	public class User
	{
		[Key]
		[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
		public int Id { get; set; }
		public string FirstName { get; set; }
                
                public virtual List<Job> jobs {get; set; }  // 1 to many		
	}
 
Job.cs
public class Job
	{
		[Key]
		[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
		public int Id { get; set; }
		public string Name { get; set; }
		public int UserId { get; set; }
		[ForeignKey("UserId")]
		public virtual User User { get; set; }
	} 



DBContext.cs 
	public class DBContext: DbContext
	{
		public DBContext(DbContextOptions<MySQLDBContext> options) : base(options) { }
		
		public DbSet<User> User { get; set; }
		public DbSet<Job> Job { get; set; }
	} 
```
{% endcode %}

{% code fullWidth="true" %}
```
// Principal (parent)
public class Blog
{
    public int Id { get; set; }
    public ICollection<Post> Posts { get; } = new List<Post>(); // Collection navigation containing dependents
}

// Dependent (child)
public class Post
{
    public int Id { get; set; }
    public int BlogId { get; set; } // Required foreign key property
    public Blog Blog { get; set; } = null!; // Required reference navigation to principal
}
```
{% endcode %}

Create a database

Run and Compilation, check tables have been generated.

{% code fullWidth="true" %}
```
// Add-Migration
// VS => NuGet Package Manager > Package Manager Console

enable-migrations               // creates a Migrations folder and Configuration.cs file
add-migration InitialCreate     // change database 
update-database                 // update to database server
```
{% endcode %}

\
&#x20;  name: AddUrltoTablePicture (for example)\
&#x20; <\<Change anything in up() function which generated>> \
&#x20;  update-database

{% code fullWidth="true" %}
```
// Error: cause cycles or multiple cascade paths

change:
    onDelete: ReferentialAction.Cascade);
 
to:
    onDelete: ReferentialAction.Restrict);
```
{% endcode %}
