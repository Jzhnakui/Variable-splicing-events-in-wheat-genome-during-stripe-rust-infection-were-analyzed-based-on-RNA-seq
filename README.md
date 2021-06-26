# 基于RNA-seq测序数据分析小麦在条锈 菌侵染过程中基因组发生的可变剪接事件
# 前言
此仓库是一位本科生的毕业设计项目，包含可直接运行的命令，常用R与linux命令，部分报错解决，所有input和output文件。是新手入门转录组和生物信息的极佳练手材料.
# 第1章 使用多个软件找出,小麦受条锈菌侵染下,不同时间的可变剪接事件
## 原始数据的选择和下载
### 数据源选择
- NCBI中SRA数据库的项目名：**PRJEB12497**
- 转录组数据链接： https://www.ncbi.nlm.nih.gov/Traces/study/?acc=PRJEB12497
#### 数据简介
1. 测序时间是2016年，最后的数据更新时间是2018年。
2. 共计**42个runs**，每个run的数据量在**2个G**左右，共**113.19 Gb**文件大小。
3. 实验设计上，包含条锈侵染**抗病小麦**和条锈侵染**感病小麦**两大组：
	- 第1组：侵染感病小麦品种**Vuka冬小麦**（Vuka冬小麦是一种 易感小麦条锈的品种），分别测序了条锈菌侵染冬小麦品种Vuka后**0，1，2，3，5，7，9，11天**和 **产孢**的时候的转录组数据；
	  - 共计**9**个时间段，每个时间段**3个重复**，3乘以9共计**27个runs**。
	- 第2组：侵染抗病小麦品**AvocetYr5**（AvocetYr5是一种 抗小麦条锈的品种），分别测序了条锈菌侵染抗病小麦品AvocetYr5后**0，1，2，3，5**天的转录组数据；
	  -  共计**5**个时间段，每个时间段**3个重复**，3乘以5共计15个runs。

### 数据下载教程

#### 放弃Aspera，用迅雷！

你可能会看到很多教程用什么Aspera下载，我也折腾了，但是真的麻烦，还一直出错。后来还是迅雷奈斯！你只要拿到下载链接，然后把下载链接粘贴到迅雷里面，点击确定，就行了，下载链接在哪呢？点开，先用一个叫做SRR1228245的run举例：

点开下面这个网址：

> https://trace.ncbi.nlm.nih.gov/Traces/sra/?run=SRR1228245#

然后点击网页中的data access

> <img src="https://haoran9982.oss-cn-beijing.aliyuncs.com/img/20210624150100.png" style="zoom:33%;" />

就可以看到这个SRA数据的下载连接了，然后直接点击的话，也能下载，但是可能会慢，也不好管理。还是复制一下，粘贴到迅雷里面。

而且迅雷可以批量添加链接，可以看到，SRR1228245是这个链接的后缀，那么只要改一下后面的后缀，就能直接下载其他的SRA了，用Emeditor编辑一下（Emeditor是一个字符处理的神器，对好几个G的大文件也能流畅处理，顺便安利一下B站的Emeditor课程:
> B站的Emeditor课程
> https://space.bilibili.com/501882526?spm_id_from=333.788.b_765f7570696e666f.1
30秒内得到所有要下载的链接。
把下面的链接，全选之后，批量粘贴到迅雷里面，就能直接下载了
```
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224553/ERR1224553.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224554/ERR1224554.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224555/ERR1224555.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224556/ERR1224556.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224557/ERR1224557.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224558/ERR1224558.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224559/ERR1224559.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224560/ERR1224560.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224561/ERR1224561.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224562/ERR1224562.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224563/ERR1224563.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224564/ERR1224564.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224565/ERR1224565.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224566/ERR1224566.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224567/ERR1224567.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224568/ERR1224568.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224569/ERR1224569.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224570/ERR1224570.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224571/ERR1224571.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224572/ERR1224572.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224573/ERR1224573.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224574/ERR1224574.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224575/ERR1224575.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224576/ERR1224576.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224577/ERR1224577.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224578/ERR1224578.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224579/ERR1224579.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224580/ERR1224580.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224581/ERR1224581.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224582/ERR1224582.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224583/ERR1224583.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224584/ERR1224584.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224585/ERR1224585.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224586/ERR1224586.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224587/ERR1224587.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224588/ERR1224588.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224589/ERR1224589.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224590/ERR1224590.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224591/ERR1224591.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224592/ERR1224592.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224593/ERR1224593.1
https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos2/sra-pub-run-7/ERR1224594/ERR1224594.1
```

反正我这里速度挺快的，这不比什么Aspera香多了！（因为我是先下载到电脑上，然后再上传到服务器，所以没有用服务器直接下载）。

> ![](https://haoran9982.oss-cn-beijing.aliyuncs.com/img/20210624150258.png)

### 原始数据的质控

我们得到的测序数据，先看下测序质量，用的是Fastqc这个软件，看完质量之后还要用Trimmomatic 软件处理下。就像你买菜一样，菜买回来了，看一下有没有烂叶子（fastqc软件），要是有虫子烂叶子，要把烂叶子摘掉（Trimmomatic 软件），摘掉菜了，检查下，看看是不是都是好的叶子了（再来一遍fastqc 软件）。但是SRA数据库不能直接进fastqc操作，要先进行数据格式的转换。

#### SRA数据转成fastqc

SRA是原始数据，但是后面的处理要用的是fasta格式的数据，所以要转换下。
所用软件：**sratoolkit**

> 官网下载链接：https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=software

官网提供了很多的平台，可以看到有linux，window，mac等等，你要在官网下了安装包，就需要你手动安装了。
软件安装成功后，要将sra转为为fastq，用到的是 sratoolkit的fastq-dump命令。具体的命令：

```linux
fastq-dump --split-files --gzip ~/ncbi/public/sra/SRR8651554.sra
```

假如读到这里，你又懵了，这是什么？命令怎么运行？什么是命令？在哪运行？我在哪？别急，我直接把全网最好的2个课程甩你脸上！

课程①：

> 【千锋】Linux云计算教程全套_650集完全入门_学完达到云计算工程师水平_哔哩哔哩_bilibili
> https://www.bilibili.com/video/BV1pz4y1D73n

这个【千锋】的课程，基本上是手把手，而且配套的文件，有虚拟机，课程笔记，导图，等等，入门linux太好了！

配套链接附件资料（笔记，虚拟机等等）我放到这个百度盘了，有需要的话，可以自己提取：

> 链接：https://pan.baidu.com/s/130OaGUm6_gLCERs_XVptMQ 
>提取码：1234 

不是说2个教程吗？对，还有一个是生信技能树的，技能树的比较高阶了，我个人感觉还是入个门再看比较好！

课程②

> 「生信技能树」2021公益课(linux基础 & conda)_哔哩哔哩_bilibili
> https://www.bilibili.com/video/BV1Yy4y117SX

课程①，很细很细，但是太长了，很多个小时，但是想搞懂生物信息，没有什么捷径，我开始的时候，看了很多比较短的教程（5个小时以下的我认为就是很短了），后来沉下来一周左右，仔细看了这套教程，linux基本上就入门了，真的，你肯定也可以的！

**好的，现在就默认你已经会用命令了，哈哈，那就接着走SRA数据转fastq数据：**

利用fastq-dump命令可以将sra文件直接转换为fastq格式，注意，如果是illumina的双末端测序，需要添加 --split-files选项，如果需要压缩格式，需要添加 --gzip选项。最终会生成SRR8651554\_1.fastq.gz，SRR8651554\_2.fastq.gz两个文件。

等一下，你是不是又有点蒙了，**illumina**是啥？什么是双末端？--split-files是什么，是命令吗？为什么又生成了两个文件？为什么后缀是.gz？

1. illumina是一个很厉害的测序公司

2. 双末端是一种测序方式，是一个非常有创意的测序方式，能显著提升测序通量，一两句话说不清楚，还是甩你脸上教程！

   > 生物信息快速入门_哔哩哔哩_bilibili
   > https://www.bilibili.com/video/BV1C4411w7jM?from=search&seid=5626931706739837324

   > 这个教程是b站的王通老师的生物信息入门，真的炒鸡棒的一门课，总共有7个小时，如果只是想先了解测序原理的话，可以先看完前7节课。

3. --split-files 还有 后面的 --gzip都是软件的选项。举个例子，回想一下你自己的房间，再回想下你父母的房间，都是房间，但是房间里的摆设是不一样的，可能你的房间会有一个书桌，还有一个台灯，因为你有学习的需求。那每个软件其实都是一个房间，同时呢，软件有很多选项命令供你选择：比如上面的--gzip是一个压缩命令，这样输出的文件是压缩过的fastq，就会节省你的硬盘空间，如果你是土豪，有很大很大的硬盘空间，那这个命令你可以不用。

   在马上要用的“择菜” 软件Trimmomatic，就有超多的选项，因为你要切割掉一些低质量的测序数据，切多长？切除的数据碱基含量多少？这些都是通过软件中的这些选项命令进行按需操作的。那怎么去找到这个软件所有的命令呢？如果你是小白，我不推荐你去官网检索文档，因为官网当然列出的是最详细的，但是太多了嘛，你可能会花费很多时间看文档，也实操不了。推荐你进入这个网址，先检索一些经验帖子和文章之类，就能很快上手操作了。

   > https://weixin.sogou.com/

   > 这个网址可以检索很多公众号的文章，微信凭借着自己的大体量，有超多的很新的经验文章炒鸡有用，同列的话，简书平台也还行吧。会魔法的话，也可以Google。

4. .gz是因为你选择了--gzip选项，如果没有此选项，那么就是你的文件后缀就是 xxx.fastq 。之所以生成两个文件，是因为是双端测序，那怎么判断你的数据是不是双端呢？看上面的王通老师的课，也是没有讲解滴~ 要看你数据源的官网，以我的数据为例，点开这个网址

   > 转录组数据链接： https://www.ncbi.nlm.nih.gov/Traces/study/?acc=PRJEB12497

   > 找到Experiment**（ctrl + f 可以在当前网页检索）**，就点击第一个吧
   >
   > <img src="https://haoran9982.oss-cn-beijing.aliyuncs.com/img/20210624135649.png" style="zoom: 25%;" />

   > 点击之后，第一行！**ERX1296765: Illumina HiSeq 2000 paired end sequencing**
   >
   > 啥意思？Illumina HiSeq 2000这个机器，进行的双末端测序，这个信息非常重要，以后的很多软件都会要选择，因为有双端，就会有单端，具体的，还是推荐你去看万通老师的课。
#### conda出场
但是一般又很少会手动自己安装软件，因为手动安装之后，基本上会出现很多依赖包的问题，导致软件无法运行。很多人用的都是conda，如果你读到这里，又看不懂了，不知道conda是啥？别急，我直接把全网最好的课程甩给你👇
> 「生信技能树」2021公益课(linux基础 & conda)_哔哩哔哩_bilibili
> https://www.bilibili.com/video/BV1Yy4y117SX

**okk，到这里就默认你已经懂了上面的文字了**

运行命令估计挺快的，原先的sra就会直接**裂开**成双份的fastq文件！

> 这里本该有个转换成功后的服务器截图的，但是由于后续的数据处理，很少用到fastq文件了，我就删除了。。。实在是太占据硬盘空间了。你现在千万别删。。。

#### 先看下数据质量（这时候还没过滤）

用的是fastqc软件，视频教程，看第5节就好。这位up是生信技能树的一位成员，思考问题的熊，同时安利下他的播客节目，网易云就有，很NICE !

> RNA-seq转录组数据分析入门实战_哔哩哔哩_bilibili
> https://www.bilibili.com/video/BV1KJ411p7WN?p=5

####  用Trimmomatic 软件进行数据过滤（开始择菜！）

Trimmomatic用的是java，服务器要是没有java，就用conda下载。

我还记得，我在刚做这个项目的时候，这个Trimmomatic的过滤都耗费我好长时间没搞定。。

处理一个run，代码：

```
java -jar trimmomatic-0.39.jar PE -phred33 -threads 8 ERR1224553_1.fastq.gz  ERR1224553_2.fastq.gz LEADING:3 TRAILING:20 SLIDINGWINDOW:4:20 AVGQUAL:20 ;
```

你直接运行这个代码绝对是错的，因为trimmomatic-0.39.jar是程序名字，你肯定要在命令里面写清楚在哪，才行，后面的-threads 8 ，意思就是用8个线程给你干活。那线程又是什么呢？线程越多就越快吗？我曾经这么认为的，但是被大佬指点了，不一定是这样的。。。

后面的`LEADING:3 TRAILING:20 SLIDINGWINDOW:4:20 AVGQUAL:20`这些，是选项，具体的解释详见这里：

> 序列剪切相关软件（cutadapt和trimmomatic） - 简书
> https://www.jianshu.com/p/190ab5bdd6c6

以及上面思考问题的熊的视频：

> RNA-seq转录组数据分析入门实战_哔哩哔哩_bilibili
> https://www.bilibili.com/video/BV1KJ411p7WN?p=5

#### 再质控一遍，用multiqc合到一起

进到检索公众号的平台：https://weixin.sogou.com/

之所以不直接贴上网址，是因为微信公众号文章的链接，分享到别的平台，6小时就会过期失效，所以要检索下文章题目。

> 检索关键字：Fastqc结果的超详细解读
>
> 原创 豆豆花花生信星球*2018-07-21*

> 检索关键字：测序数据质量控制之FastQC
> 原创 梅零落 生信菜鸟团 2017-10-23

其实呢,如果直接在浏览器里面复制粘贴的微信公众号文章的链接很长,这种链接会6小时后失效:
> ![](https://haoran9982.oss-cn-beijing.aliyuncs.com/img/20210626222949.png)
> https://mp.weixin.qq.com/s?src=11&timestamp=1624717678&ver=3154&signature=1k3uNtV32uM5rtB9Y2bY4gIrFny1SsAI6tv7OBr72pRVo2zn2pjouyWr1C8pp9u80spNfZceYagnAzBeya8G7BKRs8zv0t6kJY97gCAc0Tlx3PXRMbTh4Dne8tqRSFRD&new=1
> 
但是你把这个链接发送到微信里,然后客户端点开后,再复制链接,就得到了一个很短的此微信公众号的链接,就不会失效啦!
> ![](https://haoran9982.oss-cn-beijing.aliyuncs.com/img/20210626223155.png)
> ![](https://haoran9982.oss-cn-beijing.aliyuncs.com/img/20210626223202.png)
> https://mp.weixin.qq.com/s/25wzafFuGUfMeR00awRFkg
## 01 建立索引
### 小麦参考基因组和注释文件gtf的下载教程:
① 点开embl的网址: http://ensemblgenomes.org/
看到这个图没,这里是植物的,第一个Triticum aestivum就是小麦的基因组(小麦的基因组很大,2018年才公布)
> ![](https://haoran9982.oss-cn-beijing.aliyuncs.com/img/20210626205110.png)

② 点一下上图中的Triticum aestivum之后,相当于点开了这个网址: https://plants.ensembl.org/Triticum_aestivum/Info/Index 
往下看下,找到download(或者网页检索download)之后,对就是这个
> <img src="https://haoran9982.oss-cn-beijing.aliyuncs.com/img/20210626221403.png" style="zoom: 50%;" />

③ 然后点一下,就会直接跳转下载了,正常就会是你的浏览器开始下载,那么你下载的这个肯定是.gz后缀的压缩文件,但是仅仅压缩文件,也有4个G了,解压之后更是有13G,再考虑到网络环境,很难下载成功:
> ![](https://haoran9982.oss-cn-beijing.aliyuncs.com/img/20210626205853.png)

那怎么办呢?别急,咱们先看下网址类型,点击Download DNA sequence (FASTA)之后呢,会跳转到这个网址:
> ![](https://haoran9982.oss-cn-beijing.aliyuncs.com/img/20210626210240.png)
> **ftp://ftp.ensemblgenomes.org/pub/plants/pre/fasta/triticum_aestivum/dna/**

可以看到这个开头是ftp,这时候又要甩出一个很好的教程,b站王通老师的:
> 如何下载生物数据_哔哩哔哩_bilibili
> https://www.bilibili.com/video/BV1F4411B7fj?from=search&seid=10963334158296167899

里面会具体讲到ftp下载的知识,数据下载都要费一节课来讲解,说明这第一步走起来并不那么容易.

最后我折腾了好多,推荐你用Filezila下载,很丝滑快速!
1. Filezila在连接服务器进行文件传输的时候很常用,非常好的一个开源软件.
2. embl他也是一个服务器呀,那我连接之后应该也能下载! 是的,可以,很丝滑.

④ 其实呢,你自己的电脑也可以与embl网站进行ftp的连接,我第一次在个人资源管理器里直接进到embl文件目录的时候,惊呆了,哈哈哈哈:
1. windows下(主要我没用过mac,也不知道行不行),任意点开一个文件夹
2. 将下载链接直接粘贴进地址栏里,然后直接回车
> ![](https://haoran9982.oss-cn-beijing.aliyuncs.com/img/20210626211210.png)

> ![](https://haoran9982.oss-cn-beijing.aliyuncs.com/img/20210626211230.png)

是的,这时候想要什么就可以直接右键,然后选择复制到文件夹. 但是我当时试的时候太慢了,我就没用这种方式. 
还是用的Filezila:
下载安装好Filezila之后,直接在 主机 处的地址栏将下载链接输入后,连接就好了!
> ![](https://haoran9982.oss-cn-beijing.aliyuncs.com/img/20210626211625.png)

可以看到列出了目录和文件,先要的话,双击就能下载了!下面这个截图里面是2M的速度,这已经很慢了,因为我连接的手机热点,平时的话,会更快,而且Filezila还很稳定!
> ![](https://haoran9982.oss-cn-beijing.aliyuncs.com/img/20210626211814.png)

GTF文件`gunzip Triticum_aestivum.IWGSC.44.gtf.gz`和dna文件都在一起,顺便下载就行了.
可以看到,我都是下载到自己电脑上了,那么怎么在服务器上用呢? 对,用Filezila再传到服务器上,因为想用服务器下载的话,有很多骚操作,然后由于网络问题,根本操作不起来,被迫周转一下!
### 建立索引

先两个教程,别害怕,不是很长:

1. 下建明老师的转录组的课,现在才安利,是因为建明老师的课一般对小白不是很友好,他演示了一遍hisat2的流程,这个连接是第十集:

> 【生信技能树】转录组测序数据分析_哔哩哔哩_bilibili
> https://www.bilibili.com/video/BV12s41137HY?p=10

2. 徐周更老师的视频,他是再windows系统下的linux虚拟机搞的,练手可以,对于小麦这么庞大的基因组,会占用很多很多内存,别想着能再个人电脑上的linux子系统建立成功了! 但是可以学个流程:

> 学转录组入门生信」如何为RNA-seq分析建立HISAT2索引_哔哩哔哩 (゜-゜)つロ 干杯~-bilibili: https://www.bilibili.com/video/BV1mt411J7v8?from=search&seid=5379635453619409581

#### 我就没必要放自己的全部命令了

具体的hisat2的运行代码,本来想放自己的代码的,但是后来觉得没必要,我每个运行命令都含有一些路径,这些路径你要换成你自己的,才能运行,所以,反正你要想复制粘贴别人的命令,再改成你自己的路径,保证软件的正确运行,那我还不如直接把我参照的内份教程给你(肯定能跑通!).  我会更关注,详尽地描述我运行中踩到的一些坑,这样会对你更有帮助.  

我的简单的一些代码:
```linux
gunzip Triticum_aestivum.IWGSC.dna.toplevel.fa.gz #参考基因组来自于ensembl,解压一下.
gunzip Triticum_aestivum.IWGSC.44.gtf.gz #注释文件同样来自于ensembl,注释文件也要下载然后解压
source activate rna #启动rna环境. 
```

你可能会有个疑问,上面代码中的`source activate rna`是什么意思呢? 

举个例子吧: 毕业答辩会穿正装和皮鞋,在家里喜欢澡的时候不穿衣服,但是会穿拖鞋,哈哈. 那现在设想一个场景,你光着身子和穿着拖鞋答辩肯定不行,对吧! 回到服务器或者计算机中的环境上来说,不同的软件就对应不同的事情,比如答辩还是洗澡; 而拖鞋和皮鞋就是环境. 

拖鞋和洗澡对应,那么为某一个或者某一类软件创建一个独立的运行环境,就不会污染其他的软件运行,有时候你甚至要为某一个软件,创建一个独立的环境. 
我推荐你最好看下这个课,上面已经安利过了,要是没看的话,真的要看一下!很重要!

> 「生信技能树」2021公益课(linux基础 & conda)_哔哩哔哩_bilibili
> https://www.bilibili.com/video/BV1Yy4y117SX

这个启动rna环境是,其实就是我为转录组(rna)创建的独立的环境,这个环境运行这个转录组的项目,其他的单细胞测序运行的东西,就不要和这个rna环境有染. 目前我也算是一个入了门的小菜鸡了,目前来说,我一天甚至一上午,就可以跑完我毕设的全部流程,而实际的服务器运算时间更短,可能全部加起来也就十几分钟,我更多的时间消耗在了运行环境的管理上,依赖包,库的处理解决上. 所以环境中软件版本的管理非常重要!

#### 代码复制教程链接:

> 软件介绍之Hisat2 :https://mp.weixin.qq.com/s/5TtH1bDw8Q6q2ZvkczfBYQ
> 原创 生信技能树 生信技能树 6月11日 

#### 我刚用hisat建立索引就遇到坑了!
首先再hisat2建立索引的时候,会生成一些中间文件,有的很大很大,我跑的时候,有个中间文件达到了将近400G;
> ![](https://haoran9982.oss-cn-beijing.aliyuncs.com/img/20210626220858.png)

这没什么,主要是最后总会建立索引失败,那我是怎么发现失败了呢? 我屁颠屁颠地拿着我建立地索引去进行下一步比对地时候,一直一直是错的! 小脑瓜敏锐地感觉到,肯定是哪里不对劲! 但是,这个世界这么大,绝对有人和我遇到了一样的问题,我就Google以及百度呀啥的,各种查. 最后明白过来,是因为内存不够. 其实我刚开始不知道看hisat2输出的log信息,人家hisat2已经告诉你了,内存用尽,运行终止.... 当时还是小白,连个log都不. 高手的话,基本上都是通过各种log信息判断的!
> ![](https://haoran9982.oss-cn-beijing.aliyuncs.com/img/20210626220916.png)

还有一些国内外的朋友也踩坑了,这几个踩坑贴也可以看下,哈哈哈:
> 小麦RNA-seq利用hisat2构建index过程 - 简书
> https://www.jianshu.com/p/426b5fefe1a5
> ![](https://haoran9982.oss-cn-beijing.aliyuncs.com/img/20210626220400.png)

> hisat2-build indexing produces more than 8 output files
> https://www.biostars.org/p/288498/

最后呢,我用一个400G内存的服务器跑成功了索引,所以我的到的这个hisat2建立的索引你也是能用的,这是百度云下载链接,这样你就不用自己建立了!

#### hisat2 2.2.1建立的小麦基因组索引文件大放送!

但是,我这是hisat2 2.2.1版本建的索引,所以你想要用我的这个索引接着用hisat软件进行比对,那你的hisat2 软件必须也要是2.2.1版本,2.2.0版本不可以!
链接:

> 链接连接连接!
## 不比对直接用salmon得到表达量

用salmon软件进行表达量的计算。

