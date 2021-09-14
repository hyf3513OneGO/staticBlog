---
title: 小白也能玩转Onedrive（进阶篇）-## 无需服务器即可离线下载并自动上传到OneDrive
date: 2021-09-12 21:50:44
tags:
---
<p>小白也能玩转Onedrive（进阶篇）-## 无需服务器即可离线下载并自动上传到OneDrive</p>
<!--more-->
<h3>0x01 首先注册Heroku账号</h3>
<p>打开<a href="https://www.heroku.com/#">https://www.heroku.com/</a></p>
<p><img src="https://ftp.bmp.ovh/imgs/2020/04/4eb32d6b0b205660.png" alt="1.png" /></p>
<p>注册我就不说了 自行注册</p>
<h3>0x02 从GitHub部署到Heroku</h3>
<p>注册完成后打开<a href="https://github.com/moeik/Heroku_aria2">https://github.com/moeik/Heroku_aria2</a></p>
<p><img src="https://ftp.bmp.ovh/imgs/2020/04/a7e912a08df7182c.png" alt="2.png" /></p>
<p>点击<strong>Deploy to Heroku</strong>，然后会自动跳转到Heroku直接部署</p>
<p><img src="https://wx1.sbimg.cn/2020/04/30/1462096260.png" alt="3.png" /></p>
<h3>0x03 Rclone挂载OneDrive</h3>
<p>下载Rclone：<a href="https://naiu.icu/%E8%BD%AF%E4%BB%B6/Windows/rclone-v1.51.0-windows-amd64.zip">Downoad</a></p>
<p>解压打开如图：</p>
<p><img src="https://ftp.bmp.ovh/imgs/2020/04/0916f71bcd936a16.png" alt="4.png" /></p>
<p>打开这里的cmd</p>
<pre><code>D:\下载\rclone-v1.51.0-windows-amd64\rclone-v1.51.0-windows-amd64&gt;rclone config  //配置rclone
No remotes found - make a new one
n) New remote
s) Set configuration password
q) Quit config
n/s/q&gt; n   //新建远程挂载
name&gt; test  //名称（自定义）
Type of storage to configure.
Enter a string value. Press Enter for the default (&quot;&quot;).
Choose a number from below, or type in your own value
 1 / 1Fichier
   \ &quot;fichier&quot;
 2 / Alias for an existing remote
   \ &quot;alias&quot;
 3 / Amazon Drive
   \ &quot;amazon cloud drive&quot;
 4 / Amazon S3 Compliant Storage Provider (AWS, Alibaba, Ceph, Digital Ocean, Dreamhost, IBM COS, Minio, etc)
   \ &quot;s3&quot;
 5 / Backblaze B2
   \ &quot;b2&quot;
 6 / Box
   \ &quot;box&quot;
 7 / Cache a remote
   \ &quot;cache&quot;
 8 / Citrix Sharefile
   \ &quot;sharefile&quot;
 9 / Dropbox
   \ &quot;dropbox&quot;
10 / Encrypt/Decrypt a remote
   \ &quot;crypt&quot;
11 / FTP Connection
   \ &quot;ftp&quot;
12 / Google Cloud Storage (this is not Google Drive)
   \ &quot;google cloud storage&quot;
13 / Google Drive
   \ &quot;drive&quot;
14 / Google Photos
   \ &quot;google photos&quot;
15 / Hubic
   \ &quot;hubic&quot;
16 / In memory object storage system.
   \ &quot;memory&quot;
17 / JottaCloud
   \ &quot;jottacloud&quot;
18 / Koofr
   \ &quot;koofr&quot;
19 / Local Disk
   \ &quot;local&quot;
20 / Mail.ru Cloud
   \ &quot;mailru&quot;
21 / Mega
   \ &quot;mega&quot;
22 / Microsoft Azure Blob Storage
   \ &quot;azureblob&quot;
23 / Microsoft OneDrive
   \ &quot;onedrive&quot;
24 / OpenDrive
   \ &quot;opendrive&quot;
25 / Openstack Swift (Rackspace Cloud Files, Memset Memstore, OVH)
   \ &quot;swift&quot;
26 / Pcloud
   \ &quot;pcloud&quot;
27 / Put.io
   \ &quot;putio&quot;
28 / QingCloud Object Storage
   \ &quot;qingstor&quot;
29 / SSH/SFTP Connection
   \ &quot;sftp&quot;
30 / Sugarsync
   \ &quot;sugarsync&quot;
31 / Transparently chunk/split large files
   \ &quot;chunker&quot;
32 / Union merges the contents of several remotes
   \ &quot;union&quot;
33 / Webdav
   \ &quot;webdav&quot;
34 / Yandex Disk
   \ &quot;yandex&quot;
35 / http Connection
   \ &quot;http&quot;
36 / premiumize.me
   \ &quot;premiumizeme&quot;
Storage&gt;23           //选择OneDrive
** See help for onedrive backend at: https://rclone.org/onedrive/ **

Microsoft App Client Id
Leave blank normally.
Enter a string value. Press Enter for the default (&quot;&quot;).
client_id&gt;   //回车
Microsoft App Client Secret
Leave blank normally.
Enter a string value. Press Enter for the default (&quot;&quot;).
client_secret&gt;  //回车
Edit advanced config? (y/n)
y) Yes
n) No (default)
y/n&gt;  //回车
Remote config
Use auto config?
 * Say Y if not sure
 * Say N if you are working on a remote or headless machine
y) Yes (default)
n) No
y/n&gt;  //回车</code></pre>
<p>这时候会弹出浏览器让你登录OneDrive
这里登录你要挂载的OneDrive账号</p>
<p><img src="https://ftp.bmp.ovh/imgs/2020/04/bea23a7443572cdd.png" alt="6.png" />
看到这个页面就可以关闭浏览器了，回到cmd
<img src="https://ftp.bmp.ovh/imgs/2020/04/3720c49f9927f54f.png" alt="5.png" />
获取成功</p>
<pre><code>Choose a number from below, or type in an existing value
 1 / OneDrive Personal or Business
   \ &quot;onedrive&quot;
 2 / Root Sharepoint site
   \ &quot;sharepoint&quot;
 3 / Type in driveID
   \ &quot;driveid&quot;
 4 / Type in SiteID
   \ &quot;siteid&quot;
 5 / Search a Sharepoint site
   \ &quot;search&quot;
Your choice&gt; 1 //选择你的OneDrive版本
Found 1 drives, please select the one you want to use:
0: OneDrive (business) id=xxxxxxxxxxxxxxxxxxxxxxx
Chose drive to use:&gt;0  //这里选择0
Found drive &#039;root&#039; of type &#039;business&#039;, URL: https://fordes-my.sharepoint.com/personal/cdm_fordes_top/Documents
Is that okay?
y) Yes (default)
n) No
y/n&gt; y //y回车</code></pre>
<p><img src="https://aroins.com/usr/uploads/2020/04/4045085468.png" alt="7.png" />
这时候关闭cmd</p>
<h3>Heroku配置填写</h3>
<p>打开<code>C:\Users\Administrator\.config\rclone\rclone.conf</code>路径不一定和我的一样，<code>Administrator</code>根据你自己的路径</p>
<p>每一项后面添加/\n，如图：
<img src="https://aroins.com/usr/uploads/2020/04/2286006289.png" alt="8.png" /></p>
<p>复制配置，从<code>type = ···</code>复制到<code>drive_type = ···</code></p>
<p>粘贴到Heroku里面的RCLONE_CONFIG</p>
<p><img src="https://aroins.com/usr/uploads/2020/04/2794326387.png" alt="9.png" /></p>
<p>最后点击Deploy app创建</p>
<p>来自 <strong>@biezhi大佬</strong>的博客 <a href="https://aroins.com/67">https://aroins.com/67</a>
感谢大佬的无私分享</p>
