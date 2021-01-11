title: Ethereum Solidity的介绍和总结

date: 2020-09-13 17:03:50

categories:
- 区块链和以太坊

tags:
- block chain
- ethereum
- solidity

---

## 介绍

solidity是etherum上的智能合约编程语言，其语义类似于Javascript、C++、Python，由C++开发

<!--more-->

## IDE

最好的solidity IDE是[Remix](https://remix.ethereum.org/)，这是一个网页版的IDE。

## 文档

https://solidity.readthedocs.io/

## 语法总结
以下的总结来自于 [cryptozombies](https://cryptozombies.io/)

### 版本指令
```solidity
pragma solidity ^0.4.19;
```

### 合约
```solidity
contract HelloWorld {

}
```

### 状态变量和证书
```
uint myUnsignedInteger = 100;
```

### 结构体

```
struct Person {
  uint age;
  string name;
}
```

### 数组

```
Person[] public people;
```

### 函数

```
function eatHamburgers(string _name, uint _amount) {

}
```

### 函数的属性

- public
合约内和合约外都可以调用

- private
只能在合约内调用

- internal 
子合约可以调用父合约类的函数

- external
只能在合约外部调用

### 函数的修饰符
- view
只读取合约内的变量的函数

- pure
既不读也不写合约内的变量的函数

### 函数返回值

```
string greeting = "What's up dog";

function sayHello() public returns (string) {
  return greeting;
}
```

### 散列函数
```
keccak256("aaaab");
```
还可以用于字符串的比较，因为solidity目前没有字符串比较的函数

### 类型转换
```
uint8 a = 5;
uint b = 6;
// 将会抛出错误，因为 a * b 返回 uint, 而不是 uint8:
uint8 c = a * b;
// 我们需要将 b 转换为 uint8:
uint8 c = a * uint8(b);
```

### 事件

事件是合约通知前端的方法

```
// 这里建立事件
event IntegersAdded(uint x, uint y, uint result);

function add(uint _x, uint _y) public {
  uint result = _x + _y;
  //触发事件，通知app
  IntegersAdded(_x, _y, result);
  return result;
}
```

你的 app 前端可以监听这个事件。JavaScript 实现如下:

```
YourContract.IntegersAdded(function(error, result) {
  // 干些事
})
```

### 地址

address 适用于表示账户的类型

### mapping
容纳多个键值对的数据结构

```
//对于金融应用程序，将用户的余额保存在一个 uint类型的变量中：
mapping (address => uint) public accountBalance;
//或者可以用来通过userId 存储/查找的用户名
mapping (uint => string) userIdToName;
```

### msg.sender

这是以太坊的内置变量，用于表示调用合约的账户地址

### require

用于检查的函数，不消耗gas
```
require(ownerZombieCount[msg.sender] == 0);
```

### 继承

```
contract ZombieFeeding is ZombieFactory {
}
```

### 引入
```
import "./someothercontract.sol";

contract newContract is SomeOtherContract {

}
```

### Store

存储在区块链上的变量，需要消耗gas
状态变量（函数之外的变量）默认这种方式

### Memory

不会存储在区块链上的变量，不会消耗gas
函数内部声明的变量默认使用这种方法

但是也可以手动声明，例如处理函数内的结构体和数组。

### 接口

如果我们的合约需要和区块链上的其他的合约会话，则需先定义一个 interface (接口)。

先举一个简单的栗子。 假设在区块链上有这么一个合约：

```solidity
contract LuckyNumber {
  mapping(address => uint) numbers;

  function setNum(uint _num) public {
    numbers[msg.sender] = _num;
  }

  function getNum(address _myAddress) public view returns (uint) {
    return numbers[_myAddress];
  }
}
```

这是个很简单的合约，您可以用它存储自己的幸运号码，并将其与您的以太坊地址关联。 这样其他人就可以通过您的地址查找您的幸运号码了。

现在假设我们有一个外部合约，使用 getNum 函数可读取其中的数据。

首先，我们定义 LuckyNumber 合约的 interface ：

```
contract NumberInterface {
  function getNum(address _myAddress) public view returns (uint);
}
```

请注意，这个过程虽然看起来像在定义一个合约，但其实内里不同：

首先，我们只声明了要与之交互的函数 —— 在本例中为 getNum —— 在其中我们没有使用到任何其他的函数或状态变量。

其次，我们并没有使用大括号（{ 和 }）定义函数体，我们单单用分号（;）结束了函数声明。这使它看起来像一个合约框架。

编译器就是靠这些特征认出它是一个接口的。

在我们的 app 代码中使用这个接口，合约就知道其他合约的函数是怎样的，应该如何调用，以及可期待什么类型的返回值。

### 处理多返回值

```solidity
function multipleReturns() internal returns(uint a, uint b, uint c) {
  return (1, 2, 3);
}

function processMultipleReturns() external {
  uint a;
  uint b;
  uint c;
  // 这样来做批量赋值:
  (a, b, c) = multipleReturns();
}

// 或者如果我们只想返回其中一个变量:
function getLastReturnValue() external {
  uint c;
  // 可以对其他字段留空:
  (,,c) = multipleReturns();
}
```

### if语句

```solidity
function eatBLT(string sandwich) public {
  // 看清楚了，当我们比较字符串的时候，需要比较他们的 keccak256 哈希码
  if (keccak256(sandwich) == keccak256("BLT")) {
    eat();
  }
}
```

## 从源代码编译solidity

### Linux 平台

[参考](https://docs.soliditylang.org/en/latest/installing-solidity.html#building-from-source)

#### 依赖
- GCC,version 8+
- CMake
- Boost
- z3
- cvc4

#### 编译

```shell
mkdir build
cd build
cmake ..
make
```