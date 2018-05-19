#afree-proxy

https://github.com/5455945/545proxy.git

## 编译
```
# linux (centos7.4，树莓派3)下编译：
git clone https://github.com/akheron/jansson.git
cd jansson
git checkout -b b2.11 v2.11
sudo ./release.sh
sudo ./configure
sudo make
sudo make install
cd ..

git clone https://github.com/libuv/libuv.git
cd libuv
git checkout -b b1.20.3 v1.20.3
sudo sh autogen.sh
sudo ./configure
sudo make
sudo make check
sudo make install

git clone https://github.com/5455945/545proxy.git
cd 545proxy
make

# 运行
./afree-proxy
# 后台运行
nohup ./afree-proxy >> run.log 2>&1 &


# windows 编译,在同一级别目录下先编译好jansson/libuv,并且放到指定的lib目录下，根据CMakelists.txt存放
mkdir build & cd build
cmake -G "Visual Studio 14 2015 Win64" ..
cmake --build . --config Release
cmake --build . --config Debug

cd Release
afree-cpuminer-proxy.exe
cd ..
cd Debug
afree-cpuminer-proxy.exe
```

## 简介

The stratum proxy designed for large mining farm featuring bandwidth saving, stability improvement, and many others, which finally improve miners reward.

由afree.org推出的Stratum协议矿场代理软件，旨在为矿场主节省带宽、提升稳定性、优化性能、统计算力信息等等功能。

afree-proxy由afree.org矿池后台源码简化而来，纯C开发，兼顾性能与稳定。

afree-proxy目前提供开源版和增强免费版。用户可同步github开源版代码自行编译、修改并运维一套stratum proxy系统。请注意afree-proxy采用GNU AGPL开源协议，不管您是在afree-proxy基础上直接修改代码，或仅采用了取自afree-proxy的部分代码片段，您必须保证所有通过网络与之交互的用户都能获取到您的服务端代码。

尽管afree-proxy设计针对大型矿场，一般几十台、几百台的小矿场亦可通过使用afree-proxy获益，只不过矿场规模越大，收益越加明显。即使您只有一台矿机，通过使用afree-proxy也能获得代理提供的诸如矿池任务检查、算力统计信息等等额外功能。

甚至您也和afree pool一样运营自己的矿池服务，您可以尝试将afree-proxy置于您的矿池服务前端，体验由afree-proxy代理带来的性能和稳定性提升。


## 特性
* 大大降低矿场出口带宽需求
  * 目前大型矿场为保障挖矿的稳定进行，通常配备了上行带宽20Mb以上的专业光纤，且随着矿场规模的扩大，带宽限制的问题将一直困扰着矿场的运维工作。afree-proxy可以帮助矿场解决这一难题：通过将afree-proxy部署与矿场内的出口节点，整个矿场矿机全部指向afree-proxy，仅有后者负责与外部矿池通信。此时一般家用宽带已能满足相当规模的矿场。
  * 如果您通过afree-proxy的免费增强版本并连接至afree.org矿池进行采矿，则设置古老的拨号网络也能带动整个矿场。

* 提升矿场整体效率和安全性
  * 使用afree-proxy后，矿场内部矿机只在局域网内与afree-proxy通信，极大地缓解了网络IO延迟，减少因此给矿机性能带来的损失。
  * 同时，由于afree-proxy的部署，矿场可对内部网络部署进行优化，将出口节点限制在afree-proxy所在节点上，而把矿机对外全部隐藏，减少外部网络攻击的接触面。
  * afree-proxy完全集成了afree pool开发团队为bfgminer/cgminer提交的矿池下发任务实时校验补丁（[bfgminer](https://github.com/luke-jr/bfgminer/pull/540)，[cgminer](https://github.com/ckolivas/cgminer/pull/638)），确保矿场算力不被矿池或黑客窃取挪为它用，是矿场连接PPLNS类型矿池的绝佳选择。结合afree.org矿池提供的算力证明体系，可完全确保PPLNS/PROP等结算机制的透明性和公正性，杜绝矿池欺诈矿场的可能。

* 负载优化和算力保障
  * afree-proxy免费增强版具备的矿池优先级、权重管理，可提供完善的上游矿池管理。
  * 优先级：用户可为不同矿池设置不同的优先级，afree-proxy将自动始终保持您的算力集中在最高优先级的矿池上。当高优先级的矿池全部失效时，会自动回落到下一优先级矿池，而一旦高优先级矿池恢复工作，afree-proxy将立即将您的算力切换回去。
  * 权重：在同一优先级的矿池之间，afree-proxy将自动按照用户设置的矿池权重，保持相应比例的矿机连接到对应的矿池上。如该优先级的矿池中存在若干失效，afree-proxy将自动将矿机按余下可工作的矿池权重比例重新分配矿机连接。
  * 用户只需在配置文件中进行一次设置，之后的管理将由afree-proxy完全自动化的完成。

* 实时统计信息
  * afree-proxy免费增强版提供完善的数据统计分析和矿机监控功能，开源版则通过日志提供简单的矿池、矿机算力统计信息。

* 卓越性能
  * afree pool开发团队长期系统底层优化的经验，确保了afree-proxy具备目前最卓越的服务器端性能，极大降低代理部署的硬件成本。
  * 无需专业服务器，普通PC、笔记本、上网本、工控机、一体机即可。免费增强版甚至提供了[Android手机版本](https://github.com/4tar/afree-proxy/tree/master/bin/pro)，即可稳定带动数百台矿机，如非手机WiFi稳定性限制，过千台矿机也不存在性能瓶颈。事实上，afree pool团队测试中已经在树莓派上通过百兆有线网络成功带动了10000+台矿机！
  * afree-proxy卓越的性能将开辟矿场部署、维护新的天地。

* 多操作系统支持
  * afree-proxy除支持Linux、FreeBSD等多个常用服务器平台外，也支持Server 2008及以上的Windows版本，极大方便广大没有类UNIX平台经验的普通用户。
  * 对于不便自行编译的用户，您可以根据您使用的操作系统类型，直接到[对应目录](https://github.com/4tar/afree-proxy/tree/master/bin)下载预编译的版本，即刻开始使用！其中pro目录为免费增强版，其它目录下为开源版。

* 开源
  * afree-proxy由afree pool矿池后台服务端代码精简后开源推出。您可以：1.通过阅读afree-proxy开源版的每一行代码来理解其工作原理，并做出适合您需求的修改（再次提请您注意我们采用的是AGPL协议，所有与您修改后的服务交互的网络用户也必须获得您修改后的代码）；2.自主审核afree-proxy是否存在任何伤害您算力安全的行为。源码之前，了无秘密；算力安全，不容忽视！afree pool欢迎您将修改直接通过开源社区回馈到我们维护的主干，同时我们也接受各种定制化需求的开发，请联系dev@afree.org。


## 部署

###硬件要求

afree-proxy性能出色，故对对硬件要求不高，测试中开源版使用10年前的普通PC（Athlon X2 3800+、2G内存、板载千兆网卡），即可稳定带动5000+台矿机；而免费增强版更可用小小一枚树莓派A+（带外接USB网卡）带动5000+矿机！

####开源版硬件建议配置

 主要硬件 | 1,000矿机最低 | 10,000以内矿机推荐 | 每+10,000台矿机推荐
-----|---------|-----------------|-------------------
可用内存 | 64M | 1G | +1G
CPU | 任意x86/ARM | 主流x86/ARM四核 | +主流x86/ARM四核，建议服务器级
可用磁盘 | 1G | 10G，建议7200+RPM | +10G，建议SSD

####免费增强版硬件建议配置

 主要硬件 | 1,000矿机最低 | 10,000以内矿机推荐 | 每+10,000台矿机推荐
-----|---------|-----------------|-------------------
可用内存 | 16M | 256M | +256M
CPU | 任意x86/ARM | 任意x86/ARM | +1CPU，4核以上建议工作站级
可用磁盘 | 1G | 10G，建议7200+RPM | +10G，建议SSD

基于免费增强版，afree pool可提供服务定制，如算力切割、监控等，根据定制服务的复杂程度，以上硬件配置数值会有所变化。

我们不建议用户采用昂贵的专业服务器设备，由于afree-proxy卓越的性能，普通PC、笔记本、上网本、工控机、一体机等廉价设备将是您更佳性价比的选择。免费增强版甚至提供了Android手机/树莓派版本，如果您有意采用其它专属嵌入设备如Edision等，可联系afree pool开发团队获取进一步的免费技术支持。

鉴于一般民用硬件稳定性、可靠性不及服务器产品，我们建议同时配置至少两台代理进行负载均衡，以避免硬件故障带来的矿场损失。如在部署方面存在问题，afree pool的Linux内核和网络专家可提供免费技术支持。

需要指出的是，不论afree-proxy的性能优化对硬件要求降至多低，网络带宽的节省只能体现在矿池端，即Internet端；而对于矿机端，即矿场内网的网络带宽是无法降低的，但当前主流网络设备已经足以满足一般矿场需要，详情请参考下节数据。

###带宽要求

* Internet：2Mb+
* 内网：一般来说，平均每增加1000台矿机，内网带宽需增加10Mb。也就是说，目前最低端的100Mb交换机只要稳定可靠，带宽可以带动10000台左右矿机，超出10000台需要升级内网至千兆网络。目前企业局域网市场早已进入千兆，故带宽不存在问题。即使您使用古老的百兆网络，只要稳定可靠，矿机数量不超过10,000，也无需担心内网带宽。

###矿池要求

支持stratum协议即可。

###矿机要求

据现有矿池下发任务难度、范围，和频率等因素，一般来说，开源版支持的单台矿机算力不宜超过4-30T。如使用afree-proxy免费增强版，将自动智能识别所有矿池矿机配置，无需担心。


##问题

###收费？

全部免费。如技术支持需要上门，请用户提供来往交通及可能的住宿费用。
当然，如果您在使用中感觉到afree-proxy为您提供了些许便利，或解决了您某项切实之需，从而愿意给afree pool开发团队做出一些捐赠，请直接发往afree pool矿池的收益地址：1FJnCqzui9vKDmY7iz948qccmDdaGaCgD6。谢谢您的支持和鼓励！

###使用？

请先阅读afree-proxy托管在github上[示例配置文件](https://github.com/4tar/afree-proxy/blob/master/afree-proxy.conf)，在理解并正确设置了配置文件之后，您就可以简单地通过执行afree-proxy来使用它！我们建议您通过同步github源码自行编译，如果不具备编译条件，也可以根据您的操作系统类型直接下载[预编译版本](https://github.com/4tar/afree-proxy/tree/master/bin)，下载后请注意核对[校验码](https://github.com/4tar/afree-proxy/blob/master/bin/bin.sha256sum)是否一致，以防误下了篡改后的版本影响到您的算力安全。
使用中如有任何问题，欢迎联系supoort@afree.org获得免费技术支持。
