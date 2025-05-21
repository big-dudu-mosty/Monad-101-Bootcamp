## 第二章：Solidity 快速入门

### 一、填空题

1. Solidity 中存储成本最高的变量类型是`Storage`变量，其数据永久存储在区块链上。
2. 使用`constant`关键字声明的常量可以节省 Gas 费，其值必须在编译时确定。
3. 当合约收到不带任何数据的以太转账时，会自动触发`receive`函数。

---

### 二、选择题

4. 函数选择器(selector)的计算方法是: B
   **A)** sha3(函数签名)  
   **B)** 函数签名哈希的前 4 字节  
   **C)** 函数参数的 ABI 编码  
   **D)** 函数返回值的类型哈希

5. 以下关于 mapping 的叙述错误的是: C
   **A)** 键类型可以是任意基本类型  
   **B)** 值类型支持嵌套 mapping  
   **C)** 可以通过`length`属性获取大小  
   **D)** 无法直接遍历所有键值对

---

### 三、简答题

6. 请说明`require`、`assert`、`revert`三者的使用场景差异（从触发条件和 Gas 退还角度）

   - `require` 用户检验输入参数 和外部条件是否满足，条件不满足时会退还剩余 gas
   - `assert` 检查内部不变量和不应该发生的情况，触发时会消耗所有 gas, 不会退还 gas
   - `revert` 主动回滚交易并提供错误信息，会退还剩余 gas

7. 某合约同时继承 A 和 B 合约，两者都有`foo()`函数：

```solidity
contract C is A, B {
    function foo() override(A,B) {...}
}
```

实际执行时会调用哪个父合约的函数？为什么？

- 如果 C 的 foo 函数中调用 super.foo()，则会调用 B 合约的 foo 函数. 因为 (is A, B) B 的优先级大于 A。 这里注意 和 override 里面的顺序没有关系
- 如果 C 的 foo 函数中调用 A.foo()，则会调用 A 合约的 foo 函数。
- 如果 C 的 foo 函数中调用 B.foo()，则会调用 B 合约的 foo 函数。

1. 当使用`call`方法发送 ETH 时,以下两种写法有何本质区别？

```solidity
(1) addr.call{value: 1 ether}("")
(2) addr.transfer(1 ether)
```

call: 推荐使用的

1.  失败的时候不会抛出异常 返回 true 或者 false。 可以通过检查返回值 来判断是否抛出异常
2.  默认会转发所有可用的 gas
3.  比较灵活，可以附带数据

transfer:

1.  失败的时候会抛出异常, 回滚交易
2.  固定限制 2300 gas,仅够完成基本的转账。后续 gas 可能不够用导致转账失败
3.  更安全，专为转账设计
