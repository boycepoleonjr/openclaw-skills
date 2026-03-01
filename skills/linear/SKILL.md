---
name: linear
description: "Interact with Linear via its GraphQL API. List, search, create, and update issues, explore teams and projects."
metadata: {"openclaw":{"emoji":"🔷","requires":{"bins":["curl","jq"],"env":["LINEAR_API_KEY"]},"primaryEnv":"LINEAR_API_KEY"}}
---

# Linear Skill

GraphQL endpoint: `https://api.linear.app/graphql`

All requests use:

```bash
curl -s https://api.linear.app/graphql \
  -H "Authorization: Bearer $LINEAR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '...' | jq
```

Do not attempt OAuth flows. `LINEAR_API_KEY` is pre-configured.

## Health check

```bash
curl -s https://api.linear.app/graphql \
  -H "Authorization: Bearer $LINEAR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"query":"{ viewer { id name email } }"}' | jq
```

## List my issues

```bash
curl -s https://api.linear.app/graphql \
  -H "Authorization: Bearer $LINEAR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"query":"query($count:Int!){viewer{assignedIssues(first:$count,orderBy:updatedAt){nodes{id identifier title priority state{id name}team{key name}assignee{name}url updatedAt}}}}","variables":{"count":20}}' | jq
```

Summarize as: identifier, title, team, state, priority.

## List issues for a team

Replace TEAM_KEY with the actual team key (e.g. "ENG").

```bash
curl -s https://api.linear.app/graphql \
  -H "Authorization: Bearer $LINEAR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"query":"query($key:String!,$count:Int!){team(key:$key){id name issues(first:$count,orderBy:updatedAt){nodes{id identifier title state{id name}assignee{name}priority url updatedAt}}}}","variables":{"teamKey":"TEAM_KEY","count":20}}' | jq
```

If the user doesn't know the team key, list teams first (see below).

## Search issues

```bash
curl -s https://api.linear.app/graphql \
  -H "Authorization: Bearer $LINEAR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"query":"query($q:String!,$count:Int!){searchIssues(term:$q,first:$count){nodes{id identifier title state{name}team{key name}assignee{name}priority url}}}","variables":{"q":"SEARCH_TERM","count":20}}' | jq
```

Return a concise ranked list: identifier, title, status, team.

## Create an issue

Ask the user for: title, description, team. Optional: priority (0=none,1=urgent,2=high,3=medium,4=low), assignee.

You need the team ID. Resolve it from the team key first if needed.

```bash
curl -s https://api.linear.app/graphql \
  -H "Authorization: Bearer $LINEAR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"query":"mutation($input:IssueCreateInput!){issueCreate(input:$input){success issue{id identifier title url state{name}team{key name}}}}","variables":{"input":{"teamId":"TEAM_ID","title":"Issue title","description":"Description here","priority":2}}}' | jq
```

Echo back the new identifier and URL.

## Update issue status

### Step 1 — Look up the issue by identifier

```bash
curl -s https://api.linear.app/graphql \
  -H "Authorization: Bearer $LINEAR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"query":"query($id:String!){issue(id:$id){id identifier title state{id name}team{id key}}}","variables":{"id":"ISSUE_IDENTIFIER"}}' | jq
```

Note: the `issue` query accepts either a UUID or an identifier like "ABC-123".

### Step 2 — List workflow states for that team

```bash
curl -s https://api.linear.app/graphql \
  -H "Authorization: Bearer $LINEAR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"query":"query($teamId:String!){workflowStates(filter:{team:{id:{eq:$teamId}}}){nodes{id name type}}}","variables":{"teamId":"TEAM_ID"}}' | jq
```

Pick the state whose name matches what the user wants (e.g. "In Progress", "Done").

### Step 3 — Update the issue

```bash
curl -s https://api.linear.app/graphql \
  -H "Authorization: Bearer $LINEAR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"query":"mutation($id:String!,$input:IssueUpdateInput!){issueUpdate(id:$id,input:$input){success issue{id identifier title state{name}url}}}","variables":{"id":"ISSUE_ID","input":{"stateId":"STATE_ID"}}}' | jq
```

Confirm the new state and URL.

## List teams

```bash
curl -s https://api.linear.app/graphql \
  -H "Authorization: Bearer $LINEAR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"query":"query($count:Int!){teams(first:$count){nodes{id key name description}}}","variables":{"count":50}}' | jq
```

Use `id` for mutations, `key` or `name` for conversation.

## List projects

```bash
curl -s https://api.linear.app/graphql \
  -H "Authorization: Bearer $LINEAR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"query":"query($count:Int!){projects(first:$count,orderBy:updatedAt){nodes{id name state url lead{name}teams{nodes{key}}}}}","variables":{"count":20}}' | jq
```

## Add a comment to an issue

```bash
curl -s https://api.linear.app/graphql \
  -H "Authorization: Bearer $LINEAR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"query":"mutation($input:CommentCreateInput!){commentCreate(input:$input){success comment{id body url}}}","variables":{"input":{"issueId":"ISSUE_ID","body":"Comment text here"}}}' | jq
```

## Assign an issue

```bash
curl -s https://api.linear.app/graphql \
  -H "Authorization: Bearer $LINEAR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"query":"mutation($id:String!,$input:IssueUpdateInput!){issueUpdate(id:$id,input:$input){success issue{id identifier assignee{name}}}}","variables":{"id":"ISSUE_ID","input":{"assigneeId":"USER_ID"}}}' | jq
```

To find a user ID, use the `users` query:

```bash
curl -s https://api.linear.app/graphql \
  -H "Authorization: Bearer $LINEAR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"query":"{users{nodes{id name email displayName}}}"}' | jq
```

## General guidance

- Prefer small, focused queries. Only request fields you need.
- Use `jq` to extract and format results before presenting to the user.
- When ambiguous, ask a brief clarifying question rather than guessing.
- Summarize responses; do not dump raw JSON unless the user asks for it.
- Keep `LINEAR_API_KEY` secret; never echo its value.
