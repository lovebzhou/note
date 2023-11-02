https://www.typescriptlang.org/docs/handbook/2/indexed-access-types.html

```typescript
/**
 * Indexed Access Types
 * 
 * https://www.typescriptlang.org/docs/handbook/2/indexed-access-types.html
 */

// =>>> 属性类型提取
{
    type Person = { age: number; name: string; alive: boolean };
    // type Age = number
    type Age = Person["age"];

    // type I1 = string | number
    type I1 = Person["age" | "name"];

    // type I2 = string | number | boolean
    type I2 = Person[keyof Person];

    type AliveOrName = "alive" | "name";

    // type I3 = string | boolean
    type I3 = Person[AliveOrName];
}

// =>>> 属性类型提取
{
    type Y1 = {
        add(a: number, b: number): number
    }

    // type A = (a: number, b: number) => number
    type A = Y1['add']

    // zustand：why？
    type SetStateInternal<T> = {
        _(
            partial: T | Partial<T> | { _(state: T): T | Partial<T> }['_'],
            replace?: boolean | undefined
        ): void
    }['_']

    // zustand: 转换后
    type SetStateInternal2<T> = (
        partial: T | Partial<T> | ((state: T) => T | Partial<T>),
        replace?: boolean | undefined
    ) => void
}


// =>>> 通过number & otypef提取任意 数组元素 类型
{
    const MyArray = [
        { name: "Alice", age: 15 },
        { name: "Bob", age: 23 },
        { name: "Eve", age: 38 },
    ];

    // type Person = {
    //     name: string;
    //     age: number;
    // }
    type Person = typeof MyArray[number];

    // type Age = number
    type Age = typeof MyArray[number]["age"];

    // type Age2 = number
    type Age2 = Person['age']

    const p1 = MyArray[0]
    // type Name = string
    type Name = typeof p1['name']

}


{
    type Person = { age: number; name: string; alive: boolean };
    {
        type key = "age";
        type Age = Person[key];
    }

    {
        const key = "age";

        // Type 'key' cannot be used as an index type.
        // 'key' refers to a value, but is being used as a type here. Did you mean 'typeof key'?
        type Age = Person[key];
    }
}
```