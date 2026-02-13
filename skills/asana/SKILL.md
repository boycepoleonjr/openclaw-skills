---
name: asana
description: Manage Asana tasks, projects, and sections via REST API with PAT auth. Use when creating, updating, completing, or querying tasks, managing project boards, moving tasks between sections, or checking project status. Covers tasks, projects, sections, search, and comments.
---

# Asana

Direct REST API integration using Personal Access Token. No dependencies, no proxy — just curl.

## Auth

Token env var: `ASANA_TOKEN` (PAT format: `2/...`)

All requests use:
```
Authorization: Bearer $ASANA_TOKEN
Content-Type: application/json
```

Base URL: `https://app.asana.com/api/1.0`

## Known IDs

Read these from the workspace `.env` file if available:
- `ASANA_WORKSPACE` — workspace GID
- `ASANA_PROJECT_PERSONAL` — Personal Life board
- `ASANA_PROJECT_DEV` — Dev Projects board
- `ASANA_PROJECT_LUMINON` — Luminon Work board

## Tasks

### List tasks in a project
```
GET /projects/{project_gid}/tasks?opt_fields=name,completed,due_on,assignee.name,notes
```

### Get a task
```
GET /tasks/{task_gid}?opt_fields=name,notes,completed,due_on,assignee.name,memberships.section.name
```

### Create a task
```
POST /tasks
{
  "data": {
    "name": "Task name",
    "projects": ["PROJECT_GID"],
    "notes": "Description",
    "due_on": "2026-02-15",
    "assignee": "me"
  }
}
```

### Update a task
```
PUT /tasks/{task_gid}
{
  "data": {
    "name": "New name",
    "due_on": "2026-02-20",
    "notes": "Updated description"
  }
}
```

### Complete a task
```
PUT /tasks/{task_gid}
{"data": {"completed": true}}
```

### Delete a task
```
DELETE /tasks/{task_gid}
```

### Create a subtask
```
POST /tasks/{parent_task_gid}/subtasks
{"data": {"name": "Subtask name"}}
```

### Get subtasks
```
GET /tasks/{task_gid}/subtasks?opt_fields=name,completed
```

## Sections

### List sections in a project
```
GET /projects/{project_gid}/sections
```

### Create a section
```
POST /projects/{project_gid}/sections
{"data": {"name": "Section Name"}}
```

### Move task to a section
```
POST /sections/{section_gid}/addTask
{"data": {"task": "TASK_GID"}}
```

## Projects

### List projects in workspace
```
GET /projects?workspace={workspace_gid}&opt_fields=name,notes,due_on
```

### Get a project
```
GET /projects/{project_gid}?opt_fields=name,notes,current_status_update
```

### Create a project
```
POST /projects
{
  "data": {
    "name": "Project Name",
    "workspace": "WORKSPACE_GID",
    "default_view": "board",
    "notes": "Description"
  }
}
```

## Search

### Search tasks in workspace
```
GET /workspaces/{workspace_gid}/tasks/search?text=query&opt_fields=name,completed,due_on,assignee.name
```

Useful filters:
- `assignee.any=me` — my tasks
- `projects.any=PROJECT_GID` — in specific project
- `completed=false` — only incomplete
- `due_on.before=2026-02-28` — due before date
- `modified_at.after=2026-02-01` — recently modified
- `is_subtask=false` — exclude subtasks

## Comments

### Add a comment to a task
```
POST /tasks/{task_gid}/stories
{"data": {"text": "Comment text"}}
```

### List comments on a task
```
GET /tasks/{task_gid}/stories?opt_fields=text,created_by.name,created_at,type
```
Filter for `type: "comment"` in results.

## Pagination

Asana uses cursor-based pagination. Response includes `next_page.offset` when more results exist:
```json
{"data": [...], "next_page": {"offset": "TOKEN"}}
```
Append `&offset=TOKEN` to the next request. Use `&limit=100` for max page size.

## opt_fields

Always specify `opt_fields` to reduce payload. Common fields:
- Tasks: `name,completed,due_on,assignee.name,notes,memberships.section.name,custom_fields`
- Projects: `name,notes,due_on,current_status_update`
- Sections: `name`

## Error handling

| Status | Meaning |
|--------|---------|
| 400 | Bad request — check JSON body |
| 401 | Invalid/expired PAT |
| 403 | No permission on resource |
| 404 | GID not found |
| 429 | Rate limited — wait and retry |

## Patterns

**Creating multiple tasks:** Loop and POST individually. No batch API.

**Moving tasks between boards:** Use `POST /tasks/{gid}/addProject` and `POST /tasks/{gid}/removeProject`.

**Add task to project with section placement:**
```
POST /tasks/{task_gid}/addProject
{"data": {"project": "PROJECT_GID", "section": "SECTION_GID"}}
```

**Rich text in notes:** Use `html_notes` field with XML-valid HTML in a `<body>` wrapper. Avoid `<p>` and `<br>` — use literal newlines.
