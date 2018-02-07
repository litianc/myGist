# 因为Ethereum和Metamask，不再需要密码

> 作者：Alex Miller on March 24, 2017)
> ConsenSys 以太坊开发人员，[Grid+](https://gridplus.io) 联合创始人
>
> 翻译&校对：[litianc](https://twitter.com/BJTUTC)
>
> 阅读时长：6分钟

我为 ConsenSys 的各种客户建立了大量的概念证明(POC)，主要是他们希望利用以太坊区块链解决某些业务用例。
奇怪的是，这些系统通常采用标准的网络登录（即用户名和密码）进行设计。

我总是问自己，为什么我设计的东西这种方式，因为，毕竟，这是每个 Web 应用最尴尬的问题，同时也是 Ethereum 今天就能够解决的问题。
所以我终于放下脚步，设计出解决方案。

## JSON Web Tokens
将密码（其是哈希的客户端）提交给认证端点并且接收令牌作为返回 是登录标准Web系统（和/或使用其API）的一种非常流行的方式。
这通常称为JSON Web Token，通常在有限的时间内（几分钟到几天）有效。
[这里](https://scotch.io/tutorials/authenticate-a-node-js-api-with-json-web-tokens)有一个很好的标准实现教程。

JSON Web Token 很好也深受欢迎，但我不得不认为，在区块链上验证自己是非常容易的。
事实上，当你使用以太坊时，你总是在这样做。

如果你把一个以太坊地址（这只是你的公钥的一个sha3哈希）作为一个网站上的帐户，想要通过用你的私钥签署一个数据来证明你拥有这个帐户是非常容易的。
这些数据是任意的，可以是网站API提供的任何随机字符串。
因此，我们可以使用地址作为用户名，并绕过密码的需要。
**事实上，我们甚至不需要使用区块链来做到这一点。**

以下是用 Express 框架实现的雏形：

首先我们需要用一个私钥做一个椭圆曲线签名：

```javascript
var ethUtil = require(‘ethereumjs-util’);  // >=5.1.1

var data = ‘i am a string’;
// Elliptic curve signature must be done on the Keccak256 Sha3 hash of a piece of data.
var message = ethUtil.toBuffer(data);    
var msgHash = ethUtil.hashPersonalMessage(message);    
var sig = ethUtil.ecsign(msgHash, privateKey);    
var serialized = ethUtil.bufferToHex(this.concatSig(sig.v, sig.r, sig.s))    
return serialized
```

不要太担心这些参数是什么。
这里有一些密码学，我鼓励你阅读椭圆曲线签名。
在 [Bitcoin Wiki](https://en.bitcoin.it/wiki/Elliptic_Curve_Digital_Signature_Algorithm) 是一个很棒的启蒙地。

无论如何，一旦我们有我们的签名组件，我们可以将它们与用户的地址一起打包，并将其全部发送到认证端点。

**POST / Authenticate**

```javascript
var jwt = require(‘jsonwebtoken’);
var ethUtil = require('ethereumjs-util');
function checkSig(req, res) {
  var sig = req.sig;
  var owner = req.owner;
  // Same data as before
  var data = ‘i am a string’;
  var message = ethUtil.toBuffer(data)
  var msgHash = ethUtil.hashPersonalMessage(message)
  // Get the address of whoever signed this message  
  var signature = ethUtil.toBuffer(sig)
  var sigParams = ethUtil.fromRpcSig(signature)
  var publicKey = ethUtil.ecrecover(msgHash, sigParams.v, sigParams.r, sigParams.s)
  var sender = ethUtil.publicToAddress(publicKey)
  var addr = ethUtil.bufferToHex(sender)
 
  // Determine if it is the same address as 'owner' 
  var match = false;
  if (addr == owner) { match = true; }
  if (match) {
    // If the signature matches the owner supplied, create a
    // JSON web token for the owner that expires in 24 hours.
    var token = jwt.sign({user: req.body.addr}, ‘i am another string’,  { expiresIn: “1d” });
    res.send(200, { success: 1, token: token })
  } else {
    // If the signature doesn’t match, error out
    res.send(500, { err: ‘Signature did not match.’});
  }
}
```

所以基本上，给定一些数据，一个地址和一个椭圆曲线签名的组成部分，我们可以通过加密的方式证明这个地址属于签名数据的人。
很酷，对吧？

一旦我们满足了签名和地址匹配，我们可以为该地址服务器端签署一个JSON Web Token。
在这种情况下，令牌有效期为1天。

现在我们只需要加入一些中间件来保护任何可能正在服务或修改受保护信息的路由。

**middleware/auth.js**

```javascript
function auth(req, res, next) {
  jwt.verify(req.body.token, ‘i am another string’, function(err, decoded) {
    if (err) { res.send(500, { error: ‘Failed to authenticate token.’}); }
    else {
      req.user = decoded.user;
      next();
    };
  });
}
```

**app.js**
```javascript

app.js
// Routes
app.post(‘/UpdateData’, auth, Routes.UpdateData);
…
```
如果提供的令牌与发送请求的用户相对应，我们继续请求路由。
请注意，middleware **修改了请求**。
因为我们知道它是在我们的 middleware 中设置, 我们需要引入新的参数"user"。

**POST / updateData**
```javascript
function UpdateData(req, res) {
  // Only use the user that was set in req by auth middleware!
  var user = req.user;
  updateYourData(user, req.body.data);
  ...
}
```
我们终于得到它了！您的用户已经真正登录，但不需要密码。

### UI 部分
但是用户如何在浏览器中实际签名这些数据呢？
Metamask来拯救！
Metamask是一个简洁的扩展程序，它将 [web3](https://github.com/MetaMask/faq/blob/master/DEVELOPERS.md) 注入到浏览器窗口中。

**mycomponent.jsx**
```javascript
makeSig（dispatch）{ 
 
function toHex（s）{ 
   var hex =''; 
   for（var i = 0; i <s.length; i ++）{hex + =''+ s.charCodeAt（i）.toString（16）; } 
   return`0x $ {hex}`; 
} 
 
var data = toHex（'我是一个字符串'）; 
web3.currentProvider.sendAsync（{id：1，method：'personal_sign'，params：[web3.eth.accounts [0]，data]}，
   function（err，result）{ 
     let sig = result.result; 
     dispatch（exchange .authenticate（sig，user））
    }）
  } 
}
render（）{ 
  let {dispatch，_main：{sig}} = this.props; 
  if（Object.keys（sig）.length == 0）{this.makeSig（dispatch）; } 
  return（
   <p>我是一个网页</ p> 
  ）; 
}
```

这将触发Metamask弹出一个窗口，要求用户签名消息：

![Metamask_Screenshot_1](https://cdn-images-1.medium.com/max/1192/1*hm3c0wy4Ddt8-q17mVGI1g.png)

一旦回调被调用，它会调用以下操作：
```javascript
authenticate(sig, user) {
  return (dispatch) => {
    fetch(`${this.api}/Authenticate`, {
      method: 'POST',
      body: JSON.stringify({ owner: user, sig: sig}),
      headers: { "Content-Type": "application/json" }
    })
    .then((res) => { return res.text(); })
    .then((body) => {
      var token = JSON.parse(body).token;
      dispatch({ type: 'SET_AUTH_TOKEN', result: token})
    })
  }
}
```
一旦你在reducer中 保存了认证令牌，你可以呼叫你的认证端点。我们终于得到它了！

需要注意的是v，r和s值必须从签名恢复。
Metamask有一个签名工具模块（[signature util module](https://github.com/MetaMask/eth-sig-util)），显示签名是如何构建的。
它可以这样解构：
```javascript
var solidity_sha3 = require('solidity-sha3').default;

let hash = solidity_sha3(data);
let sig = result.result.substr(2, result.result.length);
let r = sig.substr(0, 64);
let s = sig.substr(64, 64);
let v = parseInt(sig.substr(128, 2));
```
`r` 可以是 `0` 或 `1`。
还需要注意的是，这使用[solidity-sha3模块](https://github.com/raineorshine/solidity-sha3)来确保这个哈希算法与solidity的本地哈希方法（我们对先前签名的十六进制哈希值进行哈希处理）相同。

### 生产准备
我不能强调不够，今天每一个使用 JSON Web Token 的 Web 应用程序可以很容易地利用这个。
任何具有Metamask扩展的用户都可以简单地绕过登录界面，安全性可以比即有的管理登录的安全性要高。
这意味着更少的忘记密码，更少的浪费时间，更快乐的用户群。

而且，如果您希望用户在没有中间人的情况下互相付费（或者使用其他系统的用户），或者如果您想利用以太坊的上百万个其他功能，则可以将其设置为这种方式。

今天就做出转变。
加入以太坊生态，挑战世界。

————————

译者：这是基于以太坊开发的数字身份中最基础的一环，我将用我马马虎虎的编程能力来实现一遍原作者的 POC。
有更新成果将会补充在这里。
有兴趣的读者可以关注[我的推特](https://twitter.com/BJTUTC)，一起迎接区块链/数字身份的新一轮挑战。
