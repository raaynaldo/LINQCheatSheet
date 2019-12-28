# LINQ Cheat Sheet

## Restriction

```c#
from c in context.Courses
where c.level == 1
select c;
```

## Ordering

```c#
from c in context.Courses
where c.level == 1
orderby c.Name, c.Price descending
select c;
```

## Projection

```c#
from c in context.Courses
select new { Course = c.Name, AuthorName = c.Author.Name};
```

## Grouping

```c#
from c in context.Courses
group c by c.Level into g
select g;

from c in context.Courses
group c by c.Level into g
select new { Level = g.Key, Courses = g };
```

## Inner Join

```c#
from a in context.Authors
join c in context.Courses on a.Id equals c.AuthorId
select new {Course = c.Name, Author = a.Name};
```

## Group Join (Left Join)

```c#
from a in context.Authors
join c in context.Courses on a.Id equals c.AuthorId into g
select new {Author = a.Name, Courses = c.Count()};
```

## Cross Join

```c#
from a in context.Authors
from c in context.Courses
select new { Author = a.Name, Course = c.Name };
```
