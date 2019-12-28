# LINQ Cheat Sheet

## Restriction

```c#
from c in context.Courses
where c.level == 1
select c;

//Extension Method
context.Courses.Where(c=> c.Level == 1);
```

## Ordering

```c#
from c in context.Courses
where c.level == 1
orderby c.Name, c.Price descending
select c;

//Extension Method
context.Courses
    .OrderBy(c => c.Name) //or OrderByDescending
    .ThenBy(c.Price) //or ThenByDescending
```

## Projection

```c#
from c in context.Courses
select new { Course = c.Name, AuthorName = c.Author.Name};

//Extension Method
context.Courses.Select(c=> new
{
    CourseName = c.Name,
    AuthorName = c.Author.Name // no join req
});

var tags = context.Courses.SelectMany(c => c.Tags); //Tags is ICollection.
```

## Grouping

```c#
from c in context.Courses
group c by c.Level into g
select g;

from c in context.Courses
group c by c.Level into g
select new { Level = g.Key, Courses = g };

//Extension Method
var groups = context.Courses.GroupBy(c => c.level);
```

## Inner Join

```c#
from a in context.Authors
join c in context.Courses on a.Id equals c.AuthorId
select new {Course = c.Name, Author = a.Name};

//Extension Method
context.Courses.Join(context.Author,
    c => c.AuthorId, // key on the left side
    a => a.Id, // key on the right side
    (course, author) => new // what to do once matched
        {
            CourseName = course.Name,
            AuthorName = author.Name
        });
```

## Group Join (Left Join)

```c#
from a in context.Authors
join c in context.Courses on a.Id equals c.AuthorId into g
select new {Author = a.Name, Courses = c.Count()};

//Extension Method
context.Author.GroupJoin(context.Courses,
    a => a.Id, // key on the left side
    c => c.AuthorId, // key on the right side
    (author, courses) => new // what to do once matched
    {
        Author = a.Name,
        Course = courses.Count()
    });

```

## Cross Join

```c#
from a in context.Authors
from c in context.Courses
select new { Author = a.Name, Course = c.Name };

//Extension Method
context.Authors.SelectMany(
    a => context.Courses,
    (author, course) => new
    {
        AuthorName = author.Name,
        CourseName = course.Name
    });
```

## Partitioning

```c#
context.Courses.Skip(10).Take(10);
```

## Element Operators

```c#
// throws an exception if no elements found
context.Courses.First();
context.Courses.First(c => c.Level == 1);

// returns null if no elements found
context.Courses.FirstOrDefault();

// not supported by SQL Server
context.Courses.Last();
context.Courses.LastOrDefault();

context.Courses.Single(c=> c.Id = 1);
context.Courses.SingleDefault(c=> c.Id = 1);
```

## Quantifying

```c#
bool allInLevel1 = context.Courses.All(c => c.Level == 1);
bool anyInLevel1 = context.Courses.Any(c => c.Level == 1);
```

## Aggregating

```c#
int count = context.Courses.Count();
int count = context.Courses.Count(c => c.Level == 1);

var max = context.Courses.Max(c => c.Price);
var min = context.Courses.Min(c => c.Price);
var avg = context.Courses.Average(c => c.Price);
var sum = context.Courses.Sum(c => c.Price);
```

## IQueryable

Allows queries to be extended

```c#
IQueryable<Course> courses = context.Courses; //Query will be use where
var filtered = courses.Where(c => c.Level == 1);
foreach (var course in filtered)
    Console.WriteLine(course.Name);

IEnumerable<Course> courses = context.Courses; //Query won't be use where
var filtered = courses.Where(c => c.Level == 1);
foreach (var course in filtered)
Console.WriteLine(course.Name);
```
