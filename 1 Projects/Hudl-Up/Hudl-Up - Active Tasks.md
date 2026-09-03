# 🚧 Active Engineering Tasks

```dataview
TASK
FROM "1 Projects/Hudl-Up"
WHERE !completed AND (status = "in-progress" OR file.name = "Hudl-Up - Initial Setup Spec")
GROUP BY file.link
```


