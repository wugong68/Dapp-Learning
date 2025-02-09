# 简介 
区块链作为对等网络中的一种分布式账本技术，集成了密码学、共识机制、智能合约等多种技术，提供一种新型信任体系构建方法。智能合约具有公开透明、实时更新、准确执行等显著特点，在区块链中为信息存储、交易执行和资产管理等功能的实现提供了更安全、高效、可信的方式。但是，智能合约本身仍然存在安全问题，影响了区块链技术的进一步推广使用。  
本章节介绍几种安全分析工具, 帮忙开发者在开发 solidity 合约时, 及时发现并检测其中的漏洞  

## Mythril  
Mythril是一个以太坊官方推荐的智能合约安全分析工具，使用符合执行来检测智能合约中的各种安全漏洞，在Remix、Truffle等IDE里都有集成。
其包含的安全分析模型如下,具体可参考 [链接](https://learnblockchain.cn/article/1283)  
![安全分析模型](./images/securityModule.png)  

- 安装 ( docker 方式 )  
```
docker pull mythril/myth
``` 

- 进行检查 
```shell
docker run -v ${PWD}:/contract mythril/myth analyze  /contract/bec.sol --solv 0.4.25
```

之后可以得到如下输出
```shell
❯ docker run -v ${PWD}:/contract mythril/myth analyze  /contract/Overflow_Add.sol --solv 0.4.25
==== Integer Arithmetic Bugs ====
SWC ID: 101
Severity: High
Contract: Overflow_Add
Function name: fallback
PC address: 143
Estimated Gas Usage: 5954 - 26049
The arithmetic operator can overflow.
It is possible to cause an integer overflow or underflow in the arithmetic operation.
--------------------
In file: /contract/Overflow_Add.sol:6

balance += deposit

--------------------
Initial State:

Account: [CREATOR], balance: 0x1, nonce:0, storage:{}
Account: [ATTACKER], balance: 0x0, nonce:0, storage:{}
Account: [SOMEGUY], balance: 0x0, nonce:0, storage:{}

Transaction Sequence:

Caller: [CREATOR], calldata: , value: 0x0
Caller: [ATTACKER], function: add(uint256), txdata: 0x1003e2d2ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff, value: 0x0
```

## Oyente  
这个工具比较旧了, 目前使用的 solidity 编译器版本为 0.4.21, 但作为一个安全工具, 我们还是可以去了解下如何使用它 

- 安装 && 启动 
```
docker pull luongnguyen/oyente && docker run -i -t luongnguyen/oyente
```

- 执行测试 
``` 
cd /oyente/oyente && python oyente.py -s greeter.sol
```

## Slither  
Slither是一个用Python 3编写的智能合约静态分析框架，提供如下功能：
1) 自动化漏洞检测。提供超30多项的漏洞检查模型，模型列表详见: https://github.com/crytic/slither#detectors   
2) 自动优化检测。Slither可以检测编译器遗漏的代码优化项并给出优化建议  
3) 代码理解。Slither能够绘制合约的继承拓扑图，合约方法调用关系图等，帮助开发者理解代码  
4) 辅助代码审查。用户可以通过API与Slither进行交互  

- 安装 ( docker 方式 ) 
```
docker pull trailofbits/eth-security-toolbox
```

- 登陆容器 
```
docker run -it -v ${PWD}:/contracts trailofbits/eth-security-toolbox
```

- 进行检查 
```
cd /contracts
slither Suicidal.sol --solc /usr/bin/solc-v0.5.13
```

输入结果如下 
![安全分析模型](./images/slither.png)  


## MythX  
mythX 是一个付费工具, 支持命令行, vscode 插件等形式进行分析.   
可参考 [官网](https://docs.mythx.io/) 操作指导进行操作.  
进行正确配置后, 就可以进行 solidity 文件漏洞扫描了, 这里以 vscode 为例, 扫描结果如下.  
总的来说, 毕竟是付费的, 体验还是很不错的 ^_^
![安全分析模型](./images/scanResult.png)  

## 参考链接 
https://learnblockchain.cn/eth/dev/%E5%AE%89%E5%85%A8%E5%88%86%E6%9E%90.html  
https://zhuanlan.zhihu.com/p/164693789  