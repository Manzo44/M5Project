# M5Project

Module 5 Project for CEN 3024C. It's the Module 4 Task Manager with springdoc-openapi and Swagger UI added, and every call in the API Call Sequence Checklist was run from Swagger UI instead of cURL.

## Run it

1. Make sure PostgreSQL has a `taskmanager_db` database that the `postgres` user can log into.
2. Run `M5ProjectApplication`. The tables are rebuilt and `data.sql` reloads the sample data on every start.
3. Open `http://localhost:8080/swagger-ui/index.html`. The raw OpenAPI JSON is at `http://localhost:8080/v3/api-docs`.

## Screenshots

All 22 are in `screenshots/`, numbered in the order they were run, and each one starts with a label line naming the endpoint it tested. The three the assignment asks for at minimum:

- **One User:** 06 to 09 (create, get, update, delete)
- **One Task with a Subtask:** 10 to 15, and 17 shows the task with its subtask
- **The Task-User association:** 16 assigns bob to the task, 17 shows bob on the task, and 18 shows the task on bob

01 to 03 cover the setup checks: the raw `/v3/api-docs` JSON, Swagger UI listing all four controllers, and the Schemas section showing the validation rules on the request DTOs. 22 is an extra validation failure, where a 5-character password returns 400.

## Three changes from the checklist

1. **The new user is id 6, not 1.** `data.sql` loads five users first, so the POST creates id 6. I ran the GET, PUT, and DELETE on 6, because a PUT to 1 would have renamed the seeded user alice.
2. **The task update sends `"priorityId": 3`.** The checklist body uses `"priority": { "id": 3 }`, but `UpdateTaskRequestDTO` has no `priority` field. Spring ignores the unknown field, so that body comes back 200 with `priorityLevel` set to null. With `priorityId` the task comes back HIGH (screenshot 12).
3. **The deletes run last.** The task has to exist for the subtask and association steps, so the subtask and task deletes run after them (19 to 21). The association uses seeded user 2 (bob), because user 6 was already deleted, and deleting a user who is still assigned to a task fails with a 500.
