---
title: 自建od搜索引擎
date: 2021-09-12 21:50:44
tags:
---
<p><img src="https://wx2.sbimg.cn/2020/05/14/2005141653_5.png" alt="" /></p>
<h2>引言</h2>
<p>可能很多朋友们都对<strong>OneGO网盘导航</strong>的<strong><a href="http://go.wumings.com/sites/670.html" title="OneDrive网盘搜索引擎">OneDrive网盘搜索引擎</a></strong>十分感兴趣，相信很多朋友们也想拥有一个属于自己的，可自定义数据的搜索引擎。</p>
<p>可是网上的教程大多集中于利用<strong>SearX</strong>实现<strong>聚合</strong>各个公共搜索引擎的数据源，而不是用自己爬取的数据作为数据源。</p>
<p><img src="https://s1.ax1x.com/2020/05/14/YBhdS0.png" alt="" />
而提到了如何利用自有数据建立搜索引擎的教程大多是基于<strong>ElasticSerach</strong>，对于我们<strong>轻量级使用的用户</strong>而言，这个学习成本太高，仅仅是要实现<strong>简单的搜索</strong>以及<strong>联想关键词</strong>功能就需要数以月计的时间进行学习</p>
<p>若要使用<strong>阿里云</strong>等提供开发好的的<strong>Elastic搜索服务</strong>，又会发现每个月包月的价格对于我们这些访问量不大的小站而言，十分不划算。仅仅最低档的套餐就要104元，有这钱升级一下服务器他不香吗？
<img src="https://s1.ax1x.com/2020/05/14/YBhrmF.png" alt="" /></p>
<p><strong>所以我们来尝试自建一个搜索引擎吧！！</strong></p>
<h2>需要用到的东西</h2>
<p>Linux系统的VPS*1
ps:如果你不知道这个玩意儿是啥的话，推荐看看之前的推文：</p>
<ul>
<li><a href="http://go.wumings.com/archives/899" title="小白建站指北（一）-vps，虚拟主机，挂机宝....这些概念都是个啥啊">小白建站指北（一）-vps，虚拟主机，挂机宝....这些概念都是个啥啊</a></li>
</ul>
<p><a href="http://www.xunsearch.com/" title="XunSearch开源中文全文搜索引擎项目">XunSearch开源中文全文搜索引擎项目</a></p>
<p>MySQL数据库（作为数据源）</p>
<p>一丢丢Linux运维命令的基础</p>
<h2>XunSearch介绍</h2>
<p>Xunsearch 是一个<strong>高性能、全功能的全文检索解决方案</strong>。</p>
<p>Xunsearch 旨在帮助一般开发者针对既有的海量数据，快速而方便地建立自己的全文搜索引擎。
<strong>Xunsearch 中文译名为“迅搜”</strong>，代码中的经常被缩写为 XS，既是英文名称的缩略也是中文声母缩写。 这儿的“迅”是快速的意思，至少包含了两层涵义：其一代表了搜索结果的响应能力，其二则为二次开发难度、速度。</p>
<p>可以看出，XunSearch主要分为 利用C++开发的后端用于建立索引以及搜寻索引，还有一个XunSearch PHP-SDK作为前端。</p>
<h2>部署XunSearch</h2>
<p>现在正式开始我们的教程吧</p>
<h3>安装过程</h3>
<p>先检查是否安装了后续部署需要的组件，对于后端的安装而言，至少得有GCC用于编译，以及wget用于下载
<del>一般而言这两项都是有的</del><em>（Md,还真有些精简过的系统没有）</em>
不放心的朋友们可以试试这两个命令，来检查是否安装了这些需要的组件</p>
<pre><code class="language-c">gcc -v
rpm -qa|grep &quot;wget&quot;</code></pre>
<p><img src="https://s1.ax1x.com/2020/05/14/YBhTTH.png" alt="" /></p>
<p>检查过后就可以用Linux下常用的软件安装三句话搞定了</p>
<h4>下载并解压</h4>
<pre><code class="language-c">wget http://www.xunsearch.com/download/xunsearch-full-latest.tar.bz2
tar -xjf xunsearch-full-latest.tar.bz2</code></pre>
<p><img src="https://s1.ax1x.com/2020/05/14/YBhqfI.png" alt="" /></p>
<h4>进入目录并执行安装命令</h4>
<pre><code class="language-c">cd xunsearch-full-1.4.14/      这一步要看你自己安装下来的版本是多少
sh setup.sh</code></pre>
<p>ps:这一步由于服务器性能限制，可能会执行很久，千万不要中途断开连接哦</p>
<p><img src="https://s1.ax1x.com/2020/05/14/YBhvX8.png" alt="" /></p>
<p><img src="https://s1.ax1x.com/2020/05/14/YB4Ckj.png" alt="" /></p>
<p><img src="https://s1.ax1x.com/2020/05/14/YB4enU.png" alt="" /></p>
<h4>启动后端</h4>
<pre><code class="language-c">cd  安装目录    这里填入你上一步记下的安装目录
bin/xs-ctl.sh restart</code></pre>
<h3>配置过程</h3>
<h4>（a）部署php-sdk</h4>
<p>在安装了之后，php-sdk会被释放到安装目录下的/sdk/php/目录下</p>
<p><img src="https://s1.ax1x.com/2020/05/14/YB4B9I.png" alt="" /></p>
<p>这个<strong>php-sdk呢，就相当于一个网站的源码</strong>，要部署的时候直接复制到网站根目录即可，就和一般的网站搭建过程一样，这个不是技术上的难点</p>
<h4>（b）配置文件的编写</h4>
<p>配置文件的编写是XunSearch项目的核心部分，具体每一项的含义在<strong><a href="http://www.xunsearch.com/doc/php/guide/ini.guide" title="技术文档">技术文档</a></strong>有
我这里以一个简单够用的例子来解析</p>
<pre><code>project.name = OneGoSearch  //项目名称

[id]   //字段名称
type = id    ///字段类型，此处为主键，每个项目必须要一个主键

[name1]
index = mixed     

[link1]
index = mixed

,</code></pre>
<p>这里说明几点：这些字段需要与你存入数据库中的数据的字段相同，比如说如下图</p>
<p><img src="https://s1.ax1x.com/2020/05/14/YB4WNj.png" alt="" /></p>
<p>我要能够让用户按照名字搜索到这些资源站的文件的链接，就需要索引name1，link1，还需要一个id作为唯一标识的主键。</p>
<p>然后由于name1与link1里面的内容按照各个加盟资源站不同的命名习惯，导致名字里中英文，数字以及特殊符号的表示十分复杂，所以选择mixed类型作为字段类型</p>
<p>上面这样手动编写是为了能够更清楚的看懂配置文件编写的原则</p>
<p>实际上，官方提供了<strong>简明易用的配置文件编写工具</strong>：<strong><a href="http://www.xunsearch.com/tools/iniconfig" title="Ini文件设计辅助工具">Ini文件设计辅助工具</a></strong></p>
<p>这个只需要傻瓜化的<strong>选择类型</strong>，填入<strong>字段名</strong>即可生成配置文件</p>
<p>生成好的文件，需要放入<strong>PHP-sdk中的/app</strong>文件夹，并且<strong>命名为与项目名称相同</strong>
如下图所示
<img src="https://s1.ax1x.com/2020/05/14/YB47uT.png" alt="" /></p>
<h3>（c）建立索引并测试搜索</h3>
<p>最后一步就是建立索引啦
切换到php-sdk目录下，我们以SQL数据库作为数据源导入索引</p>
<pre><code> util/Indexer.php --rebuild --source=mysql://user:passwd@localhost/database --sql="SELECT * FROM table" --project=projectname
</code></pre>
<p><img src="https://s1.ax1x.com/2020/05/14/YB5Sv6.png" alt="" />
user:数据库的用户名
passwd：数据库密码
database：数据库名称
table：存储了需要索引的信息的数据表
projectname:上一步设置的项目名称</p>
<p>我们再来测试一下搜索
能看到结果就说明测试部署完成了</p>
<p><img src="https://s1.ax1x.com/2020/05/14/YB50ZF.png" alt="" /></p>
<p>之后我们就可以到之前安放了<strong>php-sdk的网站</strong>进行搜索了
（我这个是由大佬帮忙美化过的前端，实际上XunSearch自带的前端有点丑）</p>
<p><img src="https://s1.ax1x.com/2020/05/14/YBI9Wn.png" alt="" /></p>
<p>本文首发于<a href="https://go.wumings.com" title="OneGO网盘导航">OneGO网盘导航</a>
未经授权，不允许转载</p>
