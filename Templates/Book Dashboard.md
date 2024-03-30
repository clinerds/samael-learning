#Dashboard
# Completed Books

```dataview
Table ("![|150](" + cover + ")") as Cover, author as Author, Pages as "Pages", category as "Category"
From #Book
where contains(status,"Completed")

```


# Currently Reading

```dataview
Table ("![|150](" + cover + ")") as Cover, author as Author, Pages as "Pages", category as "Category"
From #Book
where contains(status,"Reading")

```


# Upcoming Reads

```dataview
Table ("![|150](" + cover + ")") as Cover, author as Author, Pages as "Pages", category as "Category"
From #Book
where contains(status,"Upcoming")

```


