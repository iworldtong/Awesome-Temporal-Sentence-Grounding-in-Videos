# Awesome-Language-Moment-Retrieval[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

<p align="center">
  <img width="250" src="https://camo.githubusercontent.com/1131548cf666e1150ebd2a52f44776d539f06324/68747470733a2f2f63646e2e7261776769742e636f6d2f73696e647265736f726875732f617765736f6d652f6d61737465722f6d656469612f6c6f676f2e737667" "Awesome!">
</p>

A curated list of language moment retrieval and related area. :-)

## Introduce

从CVPR16开始，学术界开始关注phrase grounding（i.e. object referring），即给一个query，在image中找到找个query对应的object。2017，2018年，大家也逐渐开始关注video中类似的grounding问题，可以被总结为 Grounding Actions and Objects by Language in Videos。Grounding这个词可能不完全准确，很多论文对这个任务都有不同的定义，如Localizing Moments in Video with Natural Language、Retrieval via Natural Language Queries等等。这里统一简写为**Language Moment Retrieval**。（这里默认针对视频任务）

以下论文总结主要分成两部分：

- **Temporal Activity Localization by Language**：给定一个query（包含对activity的描述），找到对应动作（事件）的起止时间；

  <div align="center"><img height="200px" src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554267644/Awesome%20Language%20Moment%20Retrieval/TALL_-_2.png"></div>

- **Spatio-temporal object referring by language**： 给定一个query（包含对object/person的描述），在时空中找到连续的bounding box (也就是一个tube)。

  <div align="center"><img width="500px" src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554267650/Awesome%20Language%20Moment%20Retrieval/SPRL_-_4.png"></div>

## Format

Markdown format:

```markdown
- [Paper Name](link) - Author 1 et al, `Conference Year`. [[code]](link)
```

## Change Log

- Apr. 03  Just started.

## Table of Contents

- [Papers](#papers)
  - [Survey](#survey)
  - [Before](#before) - [2015](#2015) - [2016](#2016) - [2017](#2017) - [2018](#2018) - [2019](#2019)
- [Dataset](#dataset)
- [Popular Implementations](#popular-implementations)
  - [PyTorch](#pytorch)
  - [TensorFlow](#tensorflow)
  - [Other](#other)

## Papers

### Survey

- None.

### Before

- [Visual Semantic Search: Retrieving Videos via Complex Textual Queries](<https://www.cv-foundation.org/openaccess/content_cvpr_2014/papers/Lin_Visual_Semantic_Search_2014_CVPR_paper.pdf>) - Dahua Lin et al, `CVPR 2014`.

  <details>
    <summary>简介</summary>
    <img height="300px"  src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554260117/Awesome%20Language%20Moment%20Retrieval/Visual_Semantic_Search_-_1.png">
    <p>
      手工设计特征。结合appearance, motion和spatial relations等信息设计视觉特征，采用了Semantic Graph设计描述特征，将二者的匹配问题转换成了整型线性规划问题（这个策略同样在ECCV18中也可以看到）。
    </p>
  	<p>
    	基于KITTI数据集（城市道路驾驶场景），数据库较小。
    </p>
  </details>

### 2015

- None

### 2016

- None

### 2017

- [Where to Play: Retrieval of Video Segments using Natural-Language Queries](<https://arxiv.org/abs/1707.00251>) - S. Lee et al, `arXiv 2017`.

  <details>
    <summary>简介</summary>
    <img   src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554260688/Awesome%20Language%20Moment%20Retrieval/where_to_look_-_1.png">
    <p>
      “Tracking by Captioning”的思想。
    </p>
  	<p>
    	使用Densecap对视频每一帧进行描述，根据相邻图像Captions之间的相似性进行组合得到视频语义片段。
    </p>
    <p>
    	采用了两种方法比较Captions之间的相似性：Word2Vec和Skip-thoughts vector。可能因为数据集小小的原因后者效果较优。
    </p>
  </details>

- [Localizing Moments in Video with Natural Language](https://arxiv.org/abs/1708.01641) - Lisa Anne Hendricks et al, `ICCV 2017`. [[code]](<https://people.eecs.berkeley.edu/~lisa_anne/didemo.html>)

  <details>
    <summary>简介</summary>
    <img height="300px"  src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554263567/Awesome%20Language%20Moment%20Retrieval/LMVNL_-_1.png">
     <p>
    RGB与Optical Flow同时作为输入，损失函数为inter-intra video ranking loss。
    </p>
    <p>
    标了一个新数据集，DiDeMo（把video切成了连续的长度为5s的片段，即 0s-5s 是第一个片段，5s-10s是第二个...，然后为这5s的片段添加语句描述，这样做其实降低了localization的难度，退化成了一个有限集合的retrieval问题）。DiDeMo中描述句的特性主要包含三个方面：相机视角（zoom，pan，cameraman）、时间关系（after，first）和空间关系（left，bottom）。且动词所占比例较多，这种设计思想基于在定位过程中对算法行为的理解是非常重要的。
    </p>
    <p>
    Moment Context Network(MCN)对于复杂的描述仍定位困难，如“dog stops, then starts rolling around again”，如何更好的推理语言描述中的语义是一个潜在的改进方向。
    </p>
  </details>

- [TALL: Temporal Activity Localization via Language Query](https://arxiv.org/abs/1705.02101) - Jiyang Gao et al, `ICCV 2017`. [[code]](<https://github.com/jiyanggao/TALL>). 

  <details>
    <summary>简介</summary>
    <img src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554261061/Awesome%20Language%20Moment%20Retrieval/TALL_-_1.png">
    <p>
      整个流程分为三步：
  		<ul> 
  			<li>C3D生成 visual feature；</li>
  			<li>skip-thought / LSTM生成sentence embedding；</li>
  			<li>将两部分的feature融合在一起 然后生成alignment score和boundary offset。alignment score代表了输入的query和clip是否匹配，boundary offset调整了 输入clip的边界。</li>
    	</ul>
    </p>
    <p>
     数据集方面：
      <ul> 
  			<li>基于TACoS提供了Charades的语句标注，名为Charades-STA2；</li>
  			<li>新数据集，DiDeMo（把video切成了连续的长度为5s的片段，即 0s-5s 是第一个片段，5s-10s是第二个...，然后为这5s的片段添加语句描述，这样做其实降低了localization的难度，退化成了一个有限集合的retrieval问题）；</li>
  			<li>Activitynet-Caption也提供了时序的语句标注，这个数据集本来是为dense video captioning准备的，但也可以用来做language based localization这个问题。</li>
    	</ul>
    </p>
  </details>

- [Spatio-temporal Person Retrieval via Natural Language Queries](https://arxiv.org/abs/1704.07945) - M. Yamaguchi et al, `ICCV 2017`.  [[code]](<https://www.mi.t.u-tokyo.ac.jp/>)

  <details>
    <summary>简介</summary>
    <img height="250px" src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554265133/Awesome%20Language%20Moment%20Retrieval/SPRL_-_0.png">
    <p>
      本文聚焦于对视频中符合描述的人的检测，但可以方面得扩展到其他任务，如Clip Retrieval、Action Detection等。
    </p>
    <img height="300px" src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554265133/Awesome%20Language%20Moment%20Retrieval/SPRL_-_1.png">
    <p>
      模型结构如上图所示：
  		<ul> 
  			<li>检测每一帧中的人，将相关的检测框连接起来形成tubes；</li>
  			<li>提取tube features，由6个子特征（box与image的RGB、Optical Flow和C3D特征拼接而成）；<img height="250px" src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554265133/Awesome%20Language%20Moment%20Retrieval/SPRL_-_2.png"></li>
  			<li>提取description features，采用三种方法：FVs based on HGLMM、Skip-thought Vectors和RNN</li>
        <li>在DSPE损失函数的基础上又添加了一项：不同模态正样本对之间距离的总和。这样做的目的是使模型直接让正样本对之间靠的更近，实验结果也验证了该方法有效。</li>
    	</ul>
    </p>
  </details>

### 2018

- [Find and Focus: Retrieve and Localize Video Events with Natural Language Queries](<http://openaccess.thecvf.com/content_ECCV_2018/papers/Dian_SHAO_Find_and_Focus_ECCV_2018_paper.pdf>) - Dian Shao  et al, `ECCV 2018`.

  <details>
    <summary>简介</summary>
    <p>
    	港中文的工作。
    </p>
    <img height="300px"   src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554266669/Awesome%20Language%20Moment%20Retrieval/find_and_focus_-_2.png">
    <p>
      Find and Focus(FIFO)模型整体分为两个部分：
      <ul>
        <li><b>Find</b>：top-level matching(paragraph vs video)，可以非常高效地滤除数据库中不相关的视频；</li>
        <li><b>Focus</b>：part-level association，以句为单位定位视频片段。</li>
      </ul>
    </p>
  <img src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554266669/Awesome%20Language%20Moment%20Retrieval/find_and_focus_-_3.png">
    <p>
    <p>
      在定位过程中，得到双流特征后，用<b>基于语义的TAG（Temporal Actionness Grouping）</b>生成Clip Proposal，将Sentences与Clip之间的Cross-domain Matching问题转换为<b>Linear Programming</b>问题。
    </p>
      <p>
    	数据集采用ActivityNet Captions和Modified LSMDC。一些实验结果如下：
    </p>
      <img src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554266669/Awesome%20Language%20Moment%20Retrieval/find_and_focus_-_4.png">
  </details>

- [Object Referring in Videos with Language and Human Gaze](https://arxiv.org/abs/1801.01582) - A. B. Vasudevan et al, `CVPR 2018`. [[code]](<http://people.ee.ethz.ch/~arunv/ORGaze.html>). 

  <details>
    <summary>简介</summary>
    <img height="250px"   src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554266103/Awesome%20Language%20Moment%20Retrieval/ORVLHG_-_1.png">
    <p>
      主要特点是添加了观察视频时人眼的信息。
    </p>
  	<img   src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554266103/Awesome%20Language%20Moment%20Retrieval/ORVLHG_-_2.png">
    <p>
      用两个LSTM分别处理局部信息与全局信息，模型输入源较多，其中人眼图像通过GazeCapture得到视频的大致位置，并将其拼接到局部特征中去（Human Gaze）。应用在一定程度上比较受限。
    </p>
  </details>

- [Actor and Action Video Segmentation from a Sentence](<https://arxiv.org/abs/1803.07485>) - Kirill Gavrilyuk et al, `CVPR2018`.

### 2019

- [MAN: Moment Alignment Network for Natural Language Moment Retrieval via Iterative Graph Adjustment](https://arxiv.org/abs/1812.00087) - Da Zhang et al, `CVPR 2019`. 
- [Peeking into the Future: Predicting Future Person Activities and Locations in Videos](https://arxiv.org/abs/1902.03748) - Junwei Liang et al, `CVPR 2019`.

## Dataset

- [ActivityNet Captions dataset](http://cs.stanford.edu/people/ranjaykrishna/densevid/).

## Popular Implementations

### PyTorch

- None.

### TensorFlow

- None.

### Others

- None.

## Licenses

[![CC0](http://i.creativecommons.org/p/zero/1.0/88x31.png)](http://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, [Jiutong Wei](https://github.com/iworldtong) all copyright and related or neighboring rights to this work.

