---
layout: doc
title: Build a todo list
permalink: /docs/todo/
---

In this guide we will walk you through the process of creating a todo list in Volca.

Before you start, make sure you have gone through the [getting started guide](/docs/).

## Create a database migration

All database migrations are defined in the file `services/api/src/lib/db/migrations.ts`.

Add your migration to the file like this to create the todos table:

```
...
{
    name: '09_create_todos_table',
    up: (knex: Knex) => {
        return knex.schema.createTable('todos', (table) => {
        table.uuid('id').defaultTo(knex.raw('uuid_generate_v4()')).primary();
        table.uuid('project_id').notNullable().index().references('id').inTable('projects');
        table.string('title').notNullable();
        });
    },
    down: (knex: Knex) => {
        return knex.schema.dropTable('todos');
    },
...
```

Run the command `yarn migrate:latest:local` from the `services/api` folder to apply the migration and create the table.

## Create an entity

Create a new file `services/api/src/entities/todo.ts`

```
import { Model } from 'objection';

export class Todo extends Model {
  id!: string;
  title!: string;

  static get tableName() {
    return 'todos';
  }
}

```

And export it from the `services/api/src/entities/index.ts` file:

```
export * from './todo';
```

## Create a service

Create a new file `services/api/src/services/todo-service.ts`.

```
import { injectable } from 'tsyringe';
import { Todo } from '../entities';

@injectable()
export class TodoService {
  public async list(projectId: string): Promise<Todo[]> {
    return Todo.query().where({ projectId });
  }
}

```

And export it from the `services/api/src/services/index.ts` file:

```
export * from './todo-service';
```

## Create an API action

Create a new file `services/api/src/actions/todos/list-todos.ts`

```
import { container } from 'tsyringe';
import { useApiAction } from '../utils/api-action';
import { TodoService } from '../../services';
import { CustomContext } from 'src/types';

export const listTodosAction = useApiAction(async (ctx: CustomContext) => {
  const todoService = container.resolve(TodoService);

  const { projectId } = ctx.params;

  const todos = await todoService.list(projectId);

  return {
    body: { todos },
  };
});
```

And export it from a new file `services/api/src/actions/todos/index.ts`

```
export * from './list-todos';
```

## Create an API endpoint

Import the `listTodosAction` at the top of the file `services/api/src/router.ts`

```
import { listTodosAction } from './actions/todos';

```

And add a new route that lists the todos:

```
router.get('/projects/:projectId/todos', authenticationMiddleware, projectUserMiddleware, listTodosAction);

```

In this case we want to add two middlewares:

`authenticationMiddleware` that makes sure the user is logged in before returning the todos
`projectUserMiddleware` that makes sure that the user has access to the project that holds the todos

Now we are done with the backend API. Let's move on to the frontend webapp.

## Create a type for a todo

Create a new file `clients/web/src/types/todo.ts`

```
export type Todo = {
  id: string;
  title: string;
};

```

And export it from `clients/web/src/types/index.ts`

```
export * from './todo';
```

## Create a function to call the API

Import the `Todo` type at the top of `clients/web/src/lib/clients/api-client.ts`

```
import { ..., Todo, ... } from '../../types';
```

Add a type for the response body in the same file

```
type GetTodosResponse = {
  todos: Todo[];
};
```

And add a function to the `ApiClient` class in the same file

```
static async getTodos(projectId: string): Promise<User[]> {
    const { todos } = await this.handleApiError(
      this.tokenClient.get(`projects/${id}/todos`).json<GetTodosResponse>()
    );
    return users;
  }
```

## Create a hook that fetches todos

Create a new file `clients/web/src/hooks/todo-actions.ts`

```
import { useCallback } from 'react';

import { ApiClient } from '../lib/clients/api-client';
import { Todo } from '../types';
import { useApiActions } from './api-actions';

export const useTodoActions = () => {
  const { executeApiAction } = useApiActions();

  const getTodos = async (projectId: string) =>
    await executeApiAction<Todo[]>({
      action: () => ApiClient.getTodos(projectId),
      errorMessage: 'Failed to load todos',
    });

  return {
    getTodos: useCallback(getTodos, [executeApiAction]),
  };
};
```

And export it from `clients/web/src/hooks/index.ts`

```
export * from './todo-actions';
```

## Create a page that displays a table of todos

Create a new file `clients/web/src/pages/todos.tsx`

```
import { Heading, Box, SimpleGrid } from '@chakra-ui/react';
import React, { useEffect, useState } from 'react';
import { MdWork } from 'react-icons/md';

import { AuthenticatedLayout } from '../layouts';
import { PageHeading } from '../components/generic/PageHeading';
import { SoftCard } from '../components/generic/SoftCard';
import { selectedProjectState } from '../state';
import { useRecoilValue } from 'recoil';
import { useTodoActions } from '../hooks';
import { Todo } from '../types';

export const TodosPage: React.FC = () => {
  const project = useRecoilValue(selectedProjectState);
  const [todos, setTodos] = useState<Todo[]>([]);
  const { getTodos } = useTodoActions();
  useEffect(() => {
    const loadTodos = async () => {
      console.log('loading');
      console.log(project?.name);
      if (project) {
        const data = await getTodos(project.id);
        setTodos(data || []);
      }
    };
    loadTodos();
  }, [project, getTodos]);

  return (
    <AuthenticatedLayout sidebar={false}>
      <PageHeading title="Todos" icon={MdWork} />
      <Box mt={8} />
      <SimpleGrid minChildWidth="200px" width="100%" spacingX="40px" spacingY="20px">
        {todos.map((todo) => (
          <SoftCard key={todo.id}>
            <Heading as="h3" size="sm" textAlign="center">
              {todo.title}
            </Heading>
          </SoftCard>
        ))}
      </SimpleGrid>
    </AuthenticatedLayout>
  );
};

```

And export it from `clients/web/src/pages/index.tsx`

```
export * from './todos';
```

## Create a route for the page

Import the page in the file `clients/web/src/App.tsx`.

```
import {
  TodosPage,
} from './pages';
```

Add a new route that displays the page in the same file

```
<Route
  path="/projects/:id/todos"
  element={
    <AuthenticatedRoute>
      <TodosPage />
    </AuthenticatedRoute>
  }
></Route>
```

Since this page should only be available to authenticated users, we wrap it in `<AuthenticatedRoute>`.

## Create a menu item

Add a meu item to the `LinkItems` array in the file `clients/web/src/components/layout/sidebar.tsx`

```
{
    name: 'Todos',
    onClick: () => navigate(selectedProject ? `/projects/${selectedProject.id}/todos` : '/'),
    icon: MdGroups,
},
```

## Finished!

Now you should be able to navigate to your page and see any todos stored in the database. Since we have not developed a UI to create todos, you can add them to the database manually for now to see that everything is working properly
