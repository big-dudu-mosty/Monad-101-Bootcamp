## 第二章：Solidity 快速入门

### 一、填空题

1. Solidity中存储成本最高的变量类型是`storage`变量，其数据永久存储在区块链上。  
2. 使用`constant、immutable`关键字声明的常量可以节省Gas费，其值必须在编译时确定。  
3. 当合约收到不带任何数据的以太转账时，会自动触发`receive`函数。  

---

### 二、选择题

4. 函数选择器(selector)的计算方法是：  B
   **A)** sha3(函数签名)  
   **B)** 函数名哈希的前4字节  
   **C)** 函数参数的ABI编码  
   **D)** 函数返回值的类型哈希  

5. 以下关于mapping的叙述错误的是：  D
   **A)** 键类型可以是任意基本类型  
   **B)** 值类型支持嵌套mapping  
   **C)** 可以通过`length`属性获取大小  
   **D)** 无法直接遍历所有键值对  

---

### 三、简答题

6. 请说明`require`、`assert`、`revert`三者的使用场景差异（从触发条件和Gas退还角度）

   require()
      使用场景：
      验证外部输入或合约状态的前置条件（用户输入校验、权限检查、安全约束）：
      function transfer(address to, uint amount) public {
         require(amount > 0, "Amount must be positive"); // 输入校验
         require(balances[msg.sender] >= amount, "Insufficient balance"); // 状态检查
         // ...
      }
      Gas特性：
         触发时退还所有未使用的Gas
         原因是属于预期内的业务逻辑错误，鼓励开发者妥善处理
   assert()
      使用场景
      检测永远不应发生的内部错误或不变量（数学计算验证、合约状态一致性检查）：
      function divide(uint a, uint b) public pure returns(uint) {
            uint result = a / b;
            assert(b != 0); // 理论上不可能触发（Solidity 0.8+会自动检查除零）
            return result;
      }
      Gas特性：
         触发时消耗全部Gas，不退还
         原因：表示合约存在严重逻辑错误，需要紧急修复
   revert()
      使用场景
      处理复杂逻辑或自定义回滚（替代require的链式检查、结合错误类型）：
      function complexOperation(uint id) public {
            if (!isValid(id)) {
               revert InvalidOperation(id); // 自定义错误类型
            }
            // 或直接回滚
            if (block.timestamp > deadline) revert("Expired");
      }
      Gas特性：
         触发时：退换所有未使用的Gas（与require相同）
         优势：比require更灵活，可携带自定义错误数据



7. 某合约同时继承A和B合约，两者都有`foo()`函数：

```solidity
contract C is A, B {
    function foo() override(A,B) {...}
}
```

实际执行时会调用哪个父合约的函数？为什么？
1.直接调用时
C c = new C();
c.foo();  // 实际执行的是 C 合约自身的 foo() 函数
C 通过 override(A,B)明确覆盖了父合约的foo()函数，因此调用时直接执行C中的实现
关键机制：
Solidity的继承体系遵循“最派生合约优先”原则。当子合约（C）覆盖了父合约函数时，外部调用始终触发子合约版本。

2.通过super调用时（继承链回溯）
如果在C.foo()内部使用super.foo();
function foo() override(A,B) {
    super.foo(); // 这里会触发父合约的函数
}
实际调用顺序：
A.foo() -> B.foo() (按继承声明顺序从左到右执行)
原因：
Solidity 使用C3线性算法确定继承链顺序：
   继承声明 contract C is A,B 的线性化顺序为：C -> A -> B
   super 调用会按此顺序回溯：
   C -> A (下一个) -> B (最后)

8. 当使用`call`方法发送ETH时，以下两种写法有何本质区别？

```solidity
(1) addr.call{value: 1 ether}("")
(2) addr.transfer(1 ether)
```

1.call直接使用call操作码 ，transfer基于call的封装（实际最终也在使用call）
2.call直接默认传递全部剩余Gas，transfer固定传递2300 Gas（不可修改）
3.call返回(bool success,) 需要手动检查，transfer失败自动会滚(等价于require(success))
4.call 重入风险较高，transfer重入风险低