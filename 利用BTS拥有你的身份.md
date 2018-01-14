# 利用 BTS 拥有你的身份
翻译自：http://bytemaster.github.io/article/2013/08/28/Own-Your-Identity-with-BitShares/

> 阅读时长：3分钟

**谁真正拥有你的在线身份？我可以向你保证，你所感知到的任何所有权都是一种错觉，最终你的网络身份现在还没有得到你的控制。
很可能你的身份与你的电子邮件地址捆绑在一起，并且你注册的每个服务都使用电子邮件验证作为主要的识别手段。
如果你失去了你的电子邮件帐户的访问权，那么你也失去了访问你的许多其他帐户。
更糟糕的是，如果有人能够阅读你的电子邮件，那么他们就可以在你使用的每一项服务中重置你的密码，并有效地窃取你的“身份”并拿走你的钱。
最终，谷歌、苹果、雅虎和其他人拥有你的网络身份，就像你可以信任任何一家公司一样，这些公司可以被你不信任的政府武装起来。
但是，即使你对这些公司和政府有完全的信心，这种模式下提供的担保也是如此之差，以至于每天都有大量的账户受到损害。**

它是像这样的问题，bitshares社区已着眼于解决。
受比特币协议的启发，我们正在开发的技术将帮助您控制您的身份、金融和通信，同时消除大多数形式的身份盗窃。

人们普遍认为，安全性和易用性总是处于矛盾之中。
自从1990年初与 Blowfish, PGP 和其他开放的加密系统和底层算法已经在网上出名了，加密已经提供给普通计算机用户。
尽管有技术诀窍，真正安全的系统尚未普及。
这种情况是广泛的，因为加密系统是如此难以使用以至于安全成本远远超过了感知的好处，甚至在高安全性几乎是必要时也是如此。 

我来自一个非常有电脑知识的家庭，并试图与最亲近的人建立安全的电子邮件交流。
尽管发送指令、YouTube 视频和屏幕共享会话，但加密系统几乎不可能可靠地工作。
形势已经如此糟糕，连 Edward Snowden 也无法让 Glenn Greenwald 安装 PGP 加密工具，以便他们可以安全、及时地沟通。 

大多数人最终让各个公司处理他们的身份密钥，他们在所有地方都使用相同的密码。

有了这样的经验，难怪几乎没有人使用安全电子邮件，每个人都依赖“密码”访问他们最重要的信息，例如他们的银行账户。
密码被认为是最容易使用的安全方式，对于普通人来说是很容易使用的。
不幸的是，想要身份安全，你的密码必须很长，导致它一般很难被记住。
你的密码是你的身份，因为它允许任何人访问您的帐户和在网上冒充成为你。
更复杂的是，用户必须在每个服务中使用同一个密码的易用性和不同服务之间唯一、持久、难以记住的密码的安全性之间做出选择。
大多数人最终给每个公司处理他们的身份密钥，他们在各处使用相同的密码。
再一次强调我们的结论，易用性胜过安全性。 

显然，安全性问题不是加密领域的问题，而是使其易于使用的问题。
这就是 BitShares 如何结合了 Bitcoin，BitMessage，Namecoin 的技术，一些新的方法来给大家带来的加密系统的好处。
苹果公司在1984推出麦金塔电脑，承诺向我们展示为什么1984不像“1984”。
这条信息一直与我们的本心相同；我们打算履行三十年前苹果公司做出的承诺。 

使密码学易用性的主要挑战在于安全交换所谓的“公钥”。
公钥仅仅是一个非常大的随机数，它与另一个称为私钥的大随机数配对。
假如你可以和你认识的每个人交换公钥，并保留你的私钥，那么剩下的问题就比较简单了。

正如著名的1999篇论文“[为什么乔尼不能加密](http://www.gaudior.net/alma/johnny.pdf)”所演示的那样，用户很难直接保存这些信息。
理解它背后的理论时，存在一个陡峭的学习曲线，而信息最终只是“随机数据”，所以它是一个黑盒子。
不幸的是，交换信息只是问题的一部分，另一部分是验证所提供的信息。
如果有人在电子邮件中为你提供乔尼的公钥，你怎么能信任它呢？ 

The traditional solution is to use centralized Certificate Authorities that handle the complex task of verifying that the ‘owner’ of an email really does control the private key. Unfortunately, this solution has once again removed your control over your identity and given both the Certificate Authority (CA) and your e-mail provider control over your online identity. You can be sure that the NSA can forge signatures from a CA. Even without worrying about the NSA, the CA system is only as secure as the least secure company you implicitly trust.

### Introducing BitShares ID (aka BitID)

The BitShares ID system eliminates the need for a Certificate Authority and replaces it with a new kind of block-chain which is secured by proof-of-work in a manner similar to Bitcoin. When you download the BitShares client and start it up for the first time, it will join the BitShares network and start synchronizing with the global name-chain by downloading it to your local computer. You would then select a user name of your choice and the client would check to make sure it is not in use. After selecting your name, your client will starting doing the proof-of-work required to enter your name in the name-chain with everyone else. Like Bitcoin, this process takes some time to both mine and be confirmed by other peers on the network. Within a reasonable period of time your computer will have finished reserving your name and having it confirmed by the network.
From this point you are ready to start texting, emailing, and trading with your friends. To send a friend a message all you need to do is know their BitShares ID user name. This information is easily exchanged over the phone or other traditional communication channels and readily understood by everyone. Enter their name in the ‘to field’, type your message, and click send.

的bitshares ID系统消除了对认证机构的需要，一种新的块链是由类似于比特币的工作证明担保类取代它。当你下载的bitshares客户端并开始了第一次，它将加入bitshares网络并开始与全球姓名链同步下载到本地计算机。然后，您将选择您选择的用户名，客户机将检查以确保它不在使用中。在选择你的名字之后，你的客户将开始做与其他人在名字链中输入你名字所需的工作证明。和Bitcoin一样，这一过程需要一些时间，同时也需要网络上其他同事的确认。在一段合理的时间内，您的计算机将完成您的名字并由网络确认。        

从这一点开始，你就可以开始发短信，发邮件，和你的朋友进行交易了。送朋友一个消息，所有你需要做的是了解自己的bitshares ID用户名。这些信息很容易通过电话或其他传统的交流渠道进行交流，并且容易被每个人理解。在“到字段”中输入他们的名字，键入消息，然后单击发送。 

As you can see, communicating within the BitShares ecosystem will be even easier than traditional email. There is no company to sign-up with, no certificate authority to pay, and no mail server configuration to manage. Despite being even easier to use than traditional unencrypted email, it is also several orders of magnitude more secure than even the most carefully setup and configured secure mail system available today.
There are several reasons why BitShares communication is more secure than anything on the market. The first reason is that 100% of the message is encrypted, including the sender and receiver. No one is able to collect the meta data on your e-mails and use that information to discover your social network. Second, there are no trusted third parties such as e-mail providers or Certificate Authorities with the power to compromise your account and impersonate you. Third, it is so easy to use that it will be much harder for users to accidentally compromise their security.

你可以看到，内部沟通的bitshares生态系统将比传统的电子邮件更容易。没有要注册的公司，没有证书颁发机构，也没有邮件服务器配置来管理。尽管使用比传统的未加密的电子邮件更容易，也更安全的比几个数量级甚至最精心的设置和配置的安全邮件系统可用的今天。 

还有为什么bitshares沟通比市场上的任何东西更安全的几个原因。第一个原因是100%的消息是加密的，包括发送方和接收方。没有人能够收集你的电子邮件上的元数据，并利用这些信息来发现你的社交网络。其次，没有可信的第三方如电子邮件提供商或证书颁发机构的权力妥协你的账户冒充你。第三，它是如此容易使用，这将是更加困难的用户无意中危及他们的安全。 

### Integrated Wallet

In addition to putting you in control of your identity and communication, BitShares will put you in control of your money by making Bitcoin and other crypto-currencies as easy to use as Paypal while being more secure than even Bitcoin is today. This is made possible by using the BitShares ID system and communication system to automatically and securely negotiate payment address with everyone on your contact list. Users of the BitShares Wallet interface will never have to see a Bitcoin address again because sending money to someone is literally as easy as sending them an e-mail. Select a contact, enter an amount, click send.
A significant factor in the difficulty of using Bitcoin in commerce is that you must adopt a foreign pricing system and deal with very volatile prices. This makes it difficult to interpret prices and make purchasing decisions. BitShares solves this ease-of-use challenge through by creating BitAssets such as BitUSD, BitEUR or BitGold. Users will be able to use a crypto-currency denominated in a familiar unit and spend it more securely, privately, and quickly than any other system on the market. This will make crypto-currencies so easy to use and familiar that ease of use will no longer be a limiting factor to adoption.

除了把你的身份和通信控制，bitshares会让你控制你的钱通过比特币及其他虚拟货币的容易使用贝宝，甚至比今天更安全的比特币。这是通过使用bitshares ID系统和通信系统自动地协商付款地址与你联系名单上的每个人都成为可能。该bitshares钱包界面用户将不会看到一个比特币地址又因为寄钱给某人简直是易如反掌发送电子邮件。选择联系人，输入金额，单击发送。              在商业中使用比特币困难的一个重要因素是，你必须采用外国定价制度，应对非常不稳定的价格。这使得它很难解释价格和作出购买决定。bitshares解决这种易用性挑战，通过创建bitassets如BitUSD、BitEUR或bitgold。用户将能够使用一个熟悉单位的加密货币，并比市场上任何其他系统更安全、更隐蔽、更快捷地使用它。这将使加密货币如此易于使用和熟悉，易于使用将不再是一个限制因素采用。 

### Owning Your Identity

With BitShares ID and the surrounding ecosystem it will become feasible for you and Johnny to finally take control of your online identity. With relatively straightforward integration, companies can enable you to use your BitShares ID to provide ‘single-sign-on’ for the entire internet. You will no longer have to trust any business to protect your personal information and most businesses will no longer even need to collect it. The result is that identity theft and the resulting financial, credit, and reputation theft will become much less common and no longer within reach of every scammer with a phishing site.

与bitshares ID和周围的生态系统将成为可行的你和杜琪峰终于能够控制你的网上身份。相对简单的集成，企业可以让你使用你的bitshares ID提供单点登录的整个互联网。您将不再需要信任任何业务来保护您的个人信息，大多数企业将不再需要收集它。结果是，身份盗窃和由此带来的金融、信用、信誉盗窃将在每一个钓鱼网站达到骗子变得更为普通，不再。 

