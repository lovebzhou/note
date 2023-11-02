```typescript
// =>>> Awaited<Type>
{
  // type A = string
  type A = Awaited<Promise<string>>;

  // type B = number
  type B = Awaited<Promise<Promise<number>>>;

  // type C = number | boolean
  type C = Awaited<boolean | Promise<number>>;
}

// =>>> Partial<Type>
{
  interface Todo {
    title: string;
    description: string;
  }

  function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
    return { ...todo, ...fieldsToUpdate };
  }

  const todo1 = {
    title: "organize desk",
    description: "clear clutter",
  };

  const todo2 = updateTodo(todo1, {
    description: "throw out trash",
  });

  // type T2 = {
  //   title?: string | undefined;
  //   description?: string | undefined;
  // }
  type T2 = Partial<Todo>
}

// =>>> Required<Type>
{
  interface Props {
    a?: number;
    b?: string;
  }

  const obj: Props = { a: 5 };

  const obj2: Required<Props> = { a: 5 };

  // type T = {
  //   a: number;
  //   b: string;
  // }
  type T = Required<Props>
}

// =>>> Readonly<Type>
{
  interface Todo {
    title: string;
  }

  const todo: Readonly<Todo> = {
    title: "Delete inactive users",
  };

  todo.title = "Hello";

  // type T2 = {
  //   readonly title: string;
  // }
  type T2 = Readonly<Todo>
}

// =>>> Record<Keys, Type>
{
  interface CatInfo {
    age: number;
    breed: string;
  }

  type CatName = "miffy" | "boris" | "mordred";

  const cats: Record<CatName, CatInfo> = {
    miffy: { age: 10, breed: "Persian" },
    boris: { age: 5, breed: "Maine Coon" },
    mordred: { age: 16, breed: "British Shorthair" },
  };

  cats.boris;

  // type T = {
  //   miffy: CatInfo;
  //   boris: CatInfo;
  //   mordred: CatInfo;
  // }
  type T = Record<CatName, CatInfo>
}

// =>>> Pick<Type, Keys>
{
  interface Todo {
    title: string;
    description: string;
    completed: boolean;
  }

  // type TodoPreview = {
  //   title: string;
  //   completed: boolean;
  // }
  type TodoPreview = Pick<Todo, "title" | "completed">;

  const todo: TodoPreview = {
    title: "Clean room",
    completed: false,
  };

  todo;
}

// =>>> Omit<Type, Keys>
{
  interface Todo {
    title: string;
    description: string;
    completed: boolean;
    createdAt: number;
  }

  // type TodoPreview = {
  //   title: string;
  //   completed: boolean;
  //   createdAt: number;
  // }
  type TodoPreview = Omit<Todo, "description">;

  const todo: TodoPreview = {
    title: "Clean room",
    completed: false,
    createdAt: 1615544252770,
  };

  todo;

  // type TodoInfo = {
  //   title: string;
  //   description: string;
  // }
  type TodoInfo = Omit<Todo, "completed" | "createdAt">;

  const todoInfo: TodoInfo = {
    title: "Pick up kids",
    description: "Kindergarten closes at 5pm",
  };

  todoInfo;
}

// =>>> Exclude<UnionType, ExcludedMembers>
{
  type T0 = Exclude<"a" | "b" | "c", "a">;

  // type T1 = "c"
  type T1 = Exclude<"a" | "b" | "c", "a" | "b">;


  // type T2 = string | number
  type T2 = Exclude<string | number | (() => void), Function>;
}

// =>>> Extract<Type, Union>
{
  // type T0 = "a"
  type T0 = Extract<"a" | "b" | "c", "a" | "f">;

  // type T1 = () => void
  type T1 = Extract<string | number | (() => void), Function>;
}

// =>>> NonNullable<Type>
{
  // type T0 = string | number
  type T0 = NonNullable<string | number | undefined>;

  // type T1 = string[]
  type T1 = NonNullable<string[] | null | undefined>;
}


// =>>> Parameters<Type>
{
  declare function f1(arg: { a: number; b: string }): void;

  type T0 = Parameters<() => string>;

  type T1 = Parameters<(s: string) => void>;

  type T2 = Parameters<<T>(arg: T) => T>;

  type T3 = Parameters<typeof f1>;

  type T4 = Parameters<any>;

  type T5 = Parameters<never>;

  type T6 = Parameters<string>;
}


class C {
  x = 0;
  y = 0;
}
 
type CC = typeof C

type T0 = InstanceType<typeof C>;
```