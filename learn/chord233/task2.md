## 第二章：Solidity 快速入门

### 一、填空题

1. Solidity中存储成本最高的变量类型是`_storge__`变量，其数据永久存储在区块链上。  
2. 使用`_constant__`关键字声明的常量可以节省Gas费，其值必须在编译时确定。  
4. 当合约收到不带任何数据的以太转账时，会自动触发`__receive（）_`函数。  

---

### 二、选择题

4. 函数选择器(selector)的计算方法是：  B
   **A)** sha3(函数签名)  
   **B)** 函数名哈希的前4字节  
   **C)** 函数参数的ABI编码  
   **D)** 函数返回值的类型哈希  

5. 以下关于mapping的叙述错误的是：  C
   **A)** 键类型可以是任意基本类型  
   **B)** 值类型支持嵌套mapping  
   **C)** 可以通过`length`属性获取大小  
   **D)** 无法直接遍历所有键值对  

---

### 三、简答题

6. 请说明`require`、`assert`、`revert`三者的使用场景差异（从触发条件和Gas退还角度）

require用于前置条件检查，失败退还Gas。
assert用于内部一致性检查，失败不退还Gas。
revert灵活处理错误，可带信息，退还Gas。

7. 某合约同时继承A和B合约，两者都有`foo()`函数：

```solidity
contract C is A, B {
    function foo() override(A,B) {...}
}
```

实际执行时会调用哪个父合约的函数？为什么？

实际执行时会调用C合约函数，Solidity 借鉴了 Python 的方式并且使用 “C3 线性化” 强制一个由基类构成的 DAG（有向无环图）保持一个特定的顺序，继承顺序为从右到左。


8. 当使用`call`方法发送ETH时，以下两种写法有何本质区别？

```solidity
(1) addr.call{value: 1 ether}("")
(2) addr.transfer(1 ether)
```
Gas限制：​ transfer(1 ether)：​固定 2300	
         call{value: 1 ether}("")：无限制（默认全部 Gas）

错误处理：transfer(1 ether)：自动revert	
         call{value: 1 ether}("")：返回success，需手动检查