# 快节奏多人游戏实现技术和算法 (第一节): 客户端-服务器游戏架构
## 介绍
这是系列文章的第一往届，探索使多人快节奏对战游戏成为可能的技术和算法。如果你已经了解多人游戏背后的原理，你可以放心地跳到下一篇文章。-接下来是引导介绍。

开发任何类型的游戏都面临挑战；然后多人游戏却新增了一堆问题需要处理，足够有趣的是，核心问题都符合人的自觉和物理规律。

## 作弊问题
所有问题都始于作弊。

作为一个游戏开发者，你往往不关心玩家是否在单机游戏中作弊-他的行为只会影响他自己，不会影响到别人，他开心就好。一个作弊的单机玩家可能获是到游戏体验并不符合你的预期，但毕竟那已经是他的游戏，他爱怎么玩就怎么玩。

但是，多人游戏就不一样了。在任何对战游戏中，作弊玩家不只是提高了自己的游戏体验，他同时还破坏了其它玩家的体验。作为开发者，你肯定想避免这样的事情发生，因为放任作弊，最终玩家都会弃而不玩了。

你可以做很多事情防止作弊，但是最重要的（或许也是唯一至关重要的）却很简单：不要相信玩家。总是做最坏的预期-玩家一定会设法作弊的。

## 权威的服务器和哑客户端
这会引出一个看似很简单的文案-你让游戏里所有的行为都发生在你掌控的中心服务器内，客户端只是作为一个旁观者。换句话说，你的游戏客户端把用户输入（按键事件、命令等）发送到服务器，服务器运行游戏计算，然后你把计算结果返回给客户端。这通常称之为使用权威服务器，因为游戏世界发生的所有事情的权威来自于这台服务器。

当然，你的服务器也会暴露一些问题，不过这超出了本文的讨论范围，但是，使用权威服务器可以防止很大范围的攻击，比如，你并不以作弊客户端所持有玩家生命值为准，一个作弊客户端可以修改玩家本地的数据，告诉玩家他有10000%的生命值，但是服务器知道这个玩家只有10%的生命值-当这个玩家遭受攻击时，照样会死，而无视作弊客户端的看法。

同样你也不会信任玩家提供的游戏位置，如果你任了他的数据，作弊客户端可以告诉服务器“我在(0,0)”，然后一秒后“我在(20,10)”，极可能穿过了一面墙或是比其它玩家移动速度更快。相反，服务器知道玩家的位置在(10,10)，客户端通知服务器“我想向右移动一个单位”，服务器将该玩家新的位置状态设为(11,10)，然后通知玩家“你在(11,10)”：
![](http://www.gabrielgambetta.com/img/fpm1-01.png)

总结一下：游戏状态由服务器独立维护。客户端只负责把玩家行为通知服务器。服务器周期性地更新玩家状态，并把状态返回给客户端，客户端把新的状态显示在屏幕上。

## 处理网络问题
哑客户端方案处理慢节奏的回合制类型游戏没有问题，例如策略游戏或扑克等。如果网络环境是局域网，通信比较及时，也能处理快节奏的游戏。但要是用于基于互联网的快节奏游戏，它就不凑效了。

让我们以现实举例，假如你在旧金山，连接位于纽约的服务器，大约4000公里，没有什么速度比光速更快，即是在因特网上跑的字节也没法比，光速大概300000公里/秒，所以移动4000公里耗时13ms。

这听起来貌似很快，但这已经是一个比较乐观的假设了-它假设数据以光速径直传输，但在现实中，数据传输需要经过多个路由器的中转，数据包在路由器中需要被复制、校验和重路由等，这引入了一些延迟，所以不可能以光速传输。

为了便于讨论，让我们假设从客户端到服务器需要耗时50ms，这比较接近最好的情况-要是你要从纽约连接东京的服务器呢？万一发生网络拥塞呢？100、200甚至500ms的延迟并不是不常见的。

回到我们的例子，你的客户端发送一些用户输入指令到服务器（“我按下右箭头键”），服务器50ms后收到，似设服务器处理请求后直接发回给客户端，50ms后你的客户端收到新的游戏状态（“你现位于(1,0)”）。

从你的角度看，你按下右箭头键，十分之一秒内没有任何反应，然后，你的游戏角色终于向右移动一个单元，在你输入和产生效果之间的延迟听起来并不是很大，但却是能让人感知到的-当然，半秒钟的延迟就不仅仅只是让人产生感知到的效果了，而是让游戏丧失了可玩性。

## 总结
多人在线游戏非常的好玩儿，但引入了新层次的挑战，权威服务器架构可以很好地避免大多数的作弊，但是这样直接实施会让玩家感觉到游戏很迟钝。

在接下来的文章里，我们将会探索如何基于权威服务器架构建造一套游戏系统，同时还要最小化玩家感知到的延迟，达到让玩家几乎察觉不到的程度。