# 好的测试与糟糕的测试

## 好的测试

**集成风格（Integration-style）**：通过真实的接口进行测试，而不是对内部组件进行模拟（mock）。

```typescript
// GOOD：测试可观察到的行为
test("user can checkout with valid cart", async () => {
  const cart = createCart();
  cart.add(product);
  const result = await checkout(cart, paymentMethod);
  expect(result.status).toBe("confirmed");
});
```

特征：

- 测试用户或调用者关心的行为
- 仅使用公共 API
- 在内部重构时依然有效
- 描述“做什么（WHAT）”，而不是“怎么做（HOW）”
- 每个测试只有一个逻辑断言

## 糟糕的测试

**实现细节测试（Implementation-detail tests）**：与内部结构强耦合。

```typescript
// BAD：测试实现细节
test("checkout calls paymentService.process", async () => {
  const mockPayment = jest.mock(paymentService);
  await checkout(cart, payment);
  expect(mockPayment.process).toHaveBeenCalledWith(cart.total);
});
```

危险信号（Red flags）：

- 模拟（Mocking）内部协作组件
- 测试私有方法
- 对调用次数/顺序进行断言
- 在没有改变行为的重构时测试会失败
- 测试名称描述的是“怎么做（HOW）”而不是“做什么（WHAT）”
- 通过外部手段进行验证，而不是通过接口

```typescript
// BAD：绕过接口进行验证
test("createUser saves to database", async () => {
  await createUser({ name: "Alice" });
  const row = await db.query("SELECT * FROM users WHERE name = ?", ["Alice"]);
  expect(row).toBeDefined();
});

// GOOD：通过接口进行验证
test("createUser makes user retrievable", async () => {
  const user = await createUser({ name: "Alice" });
  const retrieved = await getUser(user.id);
  expect(retrieved.name).toBe("Alice");
});
```
