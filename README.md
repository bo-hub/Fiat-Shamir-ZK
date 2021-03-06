# Fiat-Shamir 零知识证明协议

## 场景介绍
一般用户在网站注册账号的时候，网站会要求用户设置一个密码，然后在后台保存这个密码，用户登录网站的时候要求再输入这个密码，与后台保存的密码进行比较，确定是否给用户登录权限，但这种时候用户的密码容易受到字典攻击，有一些安全意识的网站会加盐保存密码的hash，增加敌手穷举用户密码或撞库的难度。但用户密码保存在网站后台服务器上始终增加了泄露的风险，Fiat-Shamir零知识证明协议可以允许用户向注册的网站证明自己知道自己的密码，而不向网站泄露任何关于密码的信息。  

## Fiat-Shamir with secret password
首先Peggy（证明者，Prover）与Victor（验证者，Verifier）就公开参数（一个素数n,一个模n群的生成元g）达成共识。  
1. Peggy首先选择她的密码，然后对密码进行哈希，把结果转成整型数值x。
```
x=int(Hash(passowrd))
y=g^x mod n
```
Peggy 把y的值发给 Victor，让他保存  

2. 现在Peggy想要登录，她选择一个随机数v，计算
```
t = g^v mod n
```
然后把t的值发给Victor   

3.  Victor收到t，然后发送随机数c给Peggy  

4.  Peggy生成随机数v，计算
```
r =v -cx mod (n-1) 
```
Peggy把r发送给Victor  

5.  Victor计算
```
val=(g^r)(y^c) mod n
```
然后判断val与t是否相等，若相等，则Peggy证明了自己知道密码，然后允许Peggy登录。

## 使用  
在remix上部署 FiatShamir.sol，然后按顺序执行Step1_register，Step2_login，Step3_randomchallenge，Step45_verify

其中各个函数的输入通过 运行 contract_step1.py 计算 y，contract_step2.py 计算t,  contract_step45.py 计算r 。

fiat_shamir_1.py 为整个交互过程的参考代码 

## 参考文献
[1] Fiat, Amos, and Adi Shamir. "How to prove yourself: Practical solutions to identification and signature problems." Conference on the Theory and Application of Cryptographic Techniques. Springer, Berlin, Heidelberg, 1986.