---
published: true
layout: post
title: "Why we should avoid DateTime.UtcNow inside a Linq to Entities query?"
type: post
---



**Because we can get unexpected results! (after read this tip, will be expected results ;))**

### What is the diference between these two Linq queries?
**Query 1**

```csharp
var filterDate = DateTime.UtcNow;
ctx.Set.Where(m => m.DateTime > filterDate);
```

**Query 2**

```csharp
ctx.Set.Where(m => m.DateTime > DateTime.UtcNow);
```
The first one will generate a SQL with WHERE clause like this:

```sql
DateTime > @p__linq__1
```
Where @p__linq__1 is the value of our filterDate variable.

The second one will generate this WHERE clause:

```sql
DateTime > SysUtcDateTime()
```

### What is the problem?
Imagine that we're using the second query inside some sync algorithm in our C# code, this algorithm is very sensitive about time, now imagine that the server where our C# code is running has a difference about seconds or minutes with the database server?

**YES, UNEXPECTED RESULTS!**

### Conclusion
Linq to Entities is very smart and it is able to translate our DateTime.Now or DateTime.UtcNow to a matching command on database side.

**The important here is: we should remember that it can do this and we should use features like these with parsimony.**
