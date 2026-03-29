---
name: migrate-to-shoehorn
description: 将测试文件中的 `as` 类型断言迁移到 @total-typescript/shoehorn。当用户提及 shoehorn、希望在测试中替换 `as` 断言，或需要部分测试数据时使用。
---

# 迁移到 Shoehorn

## 为什么选择 shoehorn？

`shoehorn` 允许你在测试中传递部分数据，同时满足 TypeScript 的类型要求。它用类型安全的替代方案替换 `as` 断言。

**仅限测试代码。** 绝对不要在生产代码中使用 shoehorn。

在测试中使用 `as` 的问题：

- 习惯上被告知不应使用它
- 必须手动指定目标类型
- 故意使用错误数据时需要双重断言（`as unknown as Type`）

## 安装

```bash
npm i @total-typescript/shoehorn
```

## 迁移模式

### 仅需少量属性的大型对象

修改前：

```ts
type Request = {
  body: { id: string };
  headers: Record<string, string>;
  cookies: Record<string, string>;
  // ...其他 20 个属性
};

it("gets user by id", () => {
  // 只关心 body.id，但必须伪造整个 Request 对象
  getUser({
    body: { id: "123" },
    headers: {},
    cookies: {},
    // ...伪造所有 20 个属性
  });
});
```

修改后：

```ts
import { fromPartial } from "@total-typescript/shoehorn";

it("gets user by id", () => {
  getUser(
    fromPartial({
      body: { id: "123" },
    }),
  );
});
```

### `as Type` → `fromPartial()`

修改前：

```ts
getUser({ body: { id: "123" } } as Request);
```

修改后：

```ts
import { fromPartial } from "@total-typescript/shoehorn";

getUser(fromPartial({ body: { id: "123" } }));
```

### `as unknown as Type` → `fromAny()`

修改前：

```ts
getUser({ body: { id: 123 } } as unknown as Request); // 故意使用错误的类型
```

修改后：

```ts
import { fromAny } from "@total-typescript/shoehorn";

getUser(fromAny({ body: { id: 123 } }));
```

## 各函数使用场景

| 函数 | 使用场景 |
| --------------- | -------------------------------------------------- |
| `fromPartial()` | 传递通过类型检查的部分数据 |
| `fromAny()`     | 传递故意错误的数据（保持自动补全功能） |
| `fromExact()`   | 强制提供完整对象（后续可替换为 fromPartial） |

## 工作流程

1. **收集需求** - 询问用户：
   - 哪些测试文件中的 `as` 断言导致了问题？
   - 他们是否在处理只需关注部分属性的大型对象？
   - 他们是否需要为了测试错误情况而传递故意错误的数据？

2. **安装并迁移**：
   - [ ] 安装：`npm i @total-typescript/shoehorn`
   - [ ] 查找带有 `as` 断言的测试文件：`grep -r " as [A-Z]" --include="*.test.ts" --include="*.spec.ts"`
   - [ ] 将 `as Type` 替换为 `fromPartial()`
   - [ ] 将 `as unknown as Type` 替换为 `fromAny()`
   - [ ] 添加来自 `@total-typescript/shoehorn` 的导入
   - [ ] 运行类型检查进行验证
