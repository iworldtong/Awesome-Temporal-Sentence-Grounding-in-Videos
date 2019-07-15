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
  - [Before](#before) - [2017](#2017) - [2018](#2018) - [2019](#2019)
- [Dataset](#dataset)
- [Benchmark Results](#benchmark-results)
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
    	采用了两种方法比较Captions之间的相似性：Word2Vec和Skip-thoughts vector。可能因为数据集较小的原因后者效果较优。
    </p>
  </details>

- [Localizing Moments in Video with Natural Language](https://arxiv.org/abs/1708.01641) - Lisa Anne Hendricks et al, `ICCV 2017`. [[code]](<https://people.eecs.berkeley.edu/~lisa_anne/didemo.html>)

  <details>
    <summary>简介</summary>
    <p>
      <b>Moment Context Network(MCN)</b>
    </p>
    <img height="300px"  src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554263567/Awesome%20Language%20Moment%20Retrieval/LMVNL_-_1.png">
     <p>
    RGB与Optical Flow同时作为输入，损失函数为inter-intra video ranking loss。
    </p>
    <p>
    标了一个新数据集——DiDeMo（把video切成了连续的长度为5s的片段，即 0s-5s 是第一个片段，5s-10s是第二个...，然后为这5s的片段添加语句描述，这样做其实降低了localization的难度，退化成了一个有限集合的retrieval问题）。DiDeMo中描述句的特性主要包含三个方面：相机视角（zoom，pan，cameraman）、时间关系（after，first）和空间关系（left，bottom）。且动词所占比例较多，这种设计思想基于在定位过程中对算法行为的理解是非常重要的。
    </p>
    <p>
    Moment Context Network(MCN)对于复杂的描述仍定位困难，如“dog stops, then starts rolling around again”，如何更好的推理语言描述中的语义是一个潜在的改进方向。
    </p>
  </details>

- [TALL: Temporal Activity Localization via Language Query](https://arxiv.org/abs/1705.02101) - Jiyang Gao et al, `ICCV 2017`. [[code]](<https://github.com/jiyanggao/TALL>). 

  <details>
    <summary>简介</summary>
    <p>
      <b>Cross-modal Temporal Regression Localizer(CTRL)</b>
    </p>
    <img src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554261061/Awesome%20Language%20Moment%20Retrieval/TALL_-_1.png">
    <p>
      整个流程分为三步：
  		<ul> 
  			<li>C3D生成 visual feature；</li>
  			<li>skip-thought / LSTM生成sentence embedding；</li>
  			<li>将两部分的feature融合在一起 然后生成alignment score和boundary offset。alignment score代表了输入的query和clip是否匹配，boundary offset调整了输入clip的边界。</li>
    	</ul>
    </p>
    <p>
     数据集方面：
      <ul> 
  			<li>Charades-STA2；</li>
  			<li>DiDeMo（把video切成了连续的长度为5s的片段，即 0s-5s 是第一个片段，5s-10s是第二个...，然后为这5s的片段添加语句描述，这样做其实降低了localization的难度，退化成了一个有限集合的retrieval问题）；</li>
  			<li>Activitynet-Caption也提供了时序的语句标注，这个数据集本来是为dense video captioning准备的，但也可以用来做language based localization这个问题。</li>
    	</ul>
    </p>
  </details>

- [Spatio-temporal Person Retrieval via Natural Language Queries](https://arxiv.org/abs/1704.07945) - M. Yamaguchi et al, `ICCV 2017`.  [[code]](<https://www.mi.t.u-tokyo.ac.jp/>)

  <details>
    <summary>简介</summary>
    <img height="250px" src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554265133/Awesome%20Language%20Moment%20Retrieval/SPRL_-_0.png">
    <p>
      本文聚焦于对视频中符合描述的人的检测，但可以方便地扩展到其他任务，如Clip Retrieval、Action Detection等。
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
      <b>Find and Focus(FIFO)</b>
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

- [Temporal Modular Networks for Retrieving Complex Compositional Activities in Videos](<http://svl.stanford.edu/assets/papers/liu2018eccv.pdf>) - B. Liu et al, `ECCV2018`.

  <details>
    <summary>简介</summary>
    <p>
      <b>Temporal Modular Network(TMN)</b>
    </p>
    <p>
      Key Observation：自然语言描述中存在着基本的子结构，它对理解视频的结构起着至关重要的作用。
    </p>
    <p>
     本文使用动态组合的神经网络模块来显式地建模Activity的多种复杂的自然语言描述的Compositional Structure，不同于以前分别进行语言和视觉的嵌入。
    </p>
    <p>
      对语言描述解析成树结构，树的节点有两类：Base Nodes（单词级别）和Combine nodes(短语或句子级别)。这两类节点分别对应后面的两种Attention。模型结构如下图所示：
      <div align="center">
      <img height="400px"  src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554343778/Awesome%20Language%20Moment%20Retrieval/TMN-1.png">
    </div>
    </p>
  </details>

- [Temporally Grounding Natural Sentence in Video](<https://aclweb.org/anthology/papers/D/D18/D18-1015/>) - J. Chen et al, `EMNLP2018`.

  <details>
    <summary>简介</summary>
    <p>
      <b>Temporal Ground Network(TGN)</b>
    </p>
    <p>
      为了使语言与视觉的建模更加紧密（Fine-grained），提出了一种Frame-by-Word Interactions。同时检索视频片段也是一次性单向过程，相对于生成很多交叠的proposal的方式很高效。模型结构如下图所示：
      <div align="center">
      <img height="450px"  src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554344829/Awesome%20Language%20Moment%20Retrieval/TGN-1.png">
    </div>
    </p>
  	<p>
     主要分为三个部分：
      <ul>
        <li><b>Encoder</b>：两个LSTM分别提取视觉与语言特征；</li>
        <li><b>Interactor</b>
          <ul>
            <li><b>Frame-Specific Sentence Feature</b>：针对每一帧计算当前句子特征（对所有单词特征结合视频某帧特征加权求和，具体权值计算可查阅论文）；</li>
            <li><b>Interaction LSTM(i-LSTM)</b>：对每一时刻，拼接视觉特征与对应的句子特征作为LSTM的输入，其隐状态也参与上步中权值的计算；</li>
          </ul>
        </li>
        <li>
          <b>Grounder</b>：直接根据i-LSTM的隐状态，得到K个时间尺度在当前帧终止的Clip属于Ground-Truth的得分，这个过程是一次单向完成的，没有生成很多交叠的proposal，因此加速了定位，相对于CTRL、MCN得到了很高的FPS（实验得到验证）。
        </li>
      </ul>
    </p>
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

- [Multilevel Language and Vision Integration for Text-to-Clip Retrieval](<https://arxiv.org/abs/1804.05113>) - H. Xu et al, `AAAI2019`. [[code]](<https://github.com/VisionLearningGroup/Text-to-Clip_Retrieval>)

  <details>
    <summary>简介</summary>
    <p>
      <b>Query-guided Segment Proposal Network(QSPN)</b>
    </p>
    <p>
      主要贡献：
       <ul>
        <li>将Query嵌入到生成proposal中，得到<b>Query-guided proposals（减少计算代价）</b>；</li>
        <li>Early fusion，用LSTM建模Text与Clip之间<b>Fine-grained Similarity</b>；</li>
        <li>将Clip-to-Text作为一个辅助任务，用多任务损失函数进行训练，发现模型在两个任务上皆有提升</b>。</li>
       </ul>
      query-guided SPN结构如下图所示：
      <div align="center">
      <img  height="200px" src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554375456/Awesome%20Language%20Moment%20Retrieval/SPN-1.png">
    </div>
    </p>
  	<p>
     <ul>
        <li>上图底部为原始的SPN（Segment Proposal Network），query-guided SPN相当于加了一层Attention；</li>
       <li>通过SPN得到一系列proposal，接下来就是把Query与Proposal之间检索匹配。具体匹配得分通过两层LSTM计算：第一层为单词层级；第二层为句子层级，同时每一步都结合视觉特征。如下图所示：<div align="center"><img height="200px"   src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554375456/Awesome%20Language%20Moment%20Retrieval/SPN-2.png"></div></li>
        <li>多任务损失——在Triplet-based retrieval loss的基础之上，添加了Captioning loss。（第二层LSTM用于生成Caption，见上图底部）</li>
     </ul>
  	</p>
  </details>

- [MAN: Moment Alignment Network for Natural Language Moment Retrieval via Iterative Graph Adjustment](https://arxiv.org/abs/1812.00087) - Da Zhang et al, `CVPR 2019`. 

  <details>
    <summary>简介</summary>
    <p>
      <b>Moment Alignment Network(MAN)</b>
    </p>
  	<p>
    	首次引入GCN到定位中。
    </p>
    <p>
      本文认为此前的方法存在两个未解决问题：<b>Semantic Misalignment</b>和<b>Structural Misalignment</b>。如下图片所示：
      <div align="center"><img height="300px" src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554295680/Awesome%20Language%20Moment%20Retrieval/MAN_-_1.png"></div>
    </p>
  <p>
    整体结构：
    <div align="center"><img height="400px" src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554295680/Awesome%20Language%20Moment%20Retrieval/MAN_-_2.png"></div>
  </p>
    <p>
      主要亮点：
      <ul>
        <li>此前的方法在生成Clip时视觉信息与描述缺少结合，为了早期过滤掉与描述不相关的视觉特征，本文将描述特征做为动态滤波器，对从视频提取出的I3D特征进行滤波;</li>
        <li>为生成多尺度的的Clip，采用时序池化;</li>
  			<li>为深度挖掘Moments之间的关系解决Structural Misalignment，创新的设计了IGAN（Iterative Graph Adjustment Network），将Clip作为图的节点，残差连接、迭代地优化邻接矩阵。下图可视化显示了IGAN的有效性：<div align="center"><img height="350px" src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554295680/Awesome%20Language%20Moment%20Retrieval/MAN_-_3.png"></div></li>
    	</ul> 
    </p>
  </details>

* [Weakly Supervised Video Moment Retrieval From Text Queries](<https://arxiv.org/abs/1904.03282>) - N. C. Mithun et al, `CVPR 2019`. 

  <details>
    <summary>简介</summary>
    <p>
      <b>Text-Guided Attention(TGA)</b>
    </p>
  	<p>
    	弱监督学习，训练数据为video-text pairs。
    </p>
    <p>
      整体结构：
      <div align="center"><img  src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554295680/Awesome%20Language%20Moment%20Retrieval/TGA-1.png"></div>
    </p>
    <p>
      <ul>
        <li>分别提取特征，对视频提取frame-wise信息;</li>
        <li>使用<b>Text-Guided Attention</b>，提取Sentence-Wise Video Feature，使句子特征和与其语义相似视频帧的特征更相近。示意图如下：<div align="center"><img  src="https://res.cloudinary.com/dzu6x6nqi/image/upload/v1554295680/Awesome%20Language%20Moment%20Retrieval/TGA-2.png"></div></li>
  			<li>在测试时，使用上面的注意力得分进行定位。</li>
    	</ul> 
    </p>
  </details>
  
* [Weakly Supervised Video Moment Retrieval From Text Queries](<https://arxiv.org/abs/1904.03282>) - N. C. Mithun et al, `CVPR 2019`. 



## Dataset

- [ActivityNet Captions](http://cs.stanford.edu/people/ranjaykrishna/densevid/).
- [Charades-STA](<https://allenai.org/plato/charades/>).
- [DiDeMo](<https://github.com/LisaAnne/LocalizingMoments>).
- [TACoS](<https://www.mpi-inf.mpg.de/departments/computer-vision-and-multimodal-computing/research/vision-and-language/tacos-multi-level-corpus/>).

## Benchmark Results

<table>
	<tr>
		<th style="text-align:center">Method</th>
		<th colspan=3 style="text-align:center">ActivityNet<br>Captions</th>
		<th colspan=3 style="text-align:center">Charades-STA</th>
		<th colspan=3 style="text-align:center">DiDeMo</th>
		<th colspan=3 style="text-align:center">TACoS</th>
	</tr>
	<tr>
		<td align=center> </td>
		<td align=center>R@1</td><td align=center>R@5</td><td align=center>R@10</td>
		<td align=center>R@1</td><td align=center>R@5</td><td align=center>R@10</td>
		<td align=center>R@1</td><td align=center>R@5</td><td align=center>mIoU</td>
		<td align=center>R@1</td><td align=center>R@5</td><td align=center>mIoU</td>
	</tr>
	<tr>
		<td>MCN</td>
		<td align=center>-</td><td align=center>-</td><td align=center>-</td>
		<td align=center>-</td><td align=center>-</td><td align=center>-</td>
    <td align=center>28.10</td><td align=center>78.21</td><td align=center>41.08</td>
    <td align=center>-</td><td align=center>-</td><td align=center>-</td>
	</tr>
	<tr>
		<td>CTRL</td>
		<td align=center>-</td><td align=center>-</td><td align=center>-</td>
		<td align=center>23.63</td><td align=center>58.92</td><td align=center>-</td>
    <td align=center>-</td><td align=center>-</td><td align=center>-</td>
    <td align=center>13.30</td><td align=center>25.42</td><td align=center>-</td>
	</tr>
  <tr>
		<td>FIFO</td>
		<td align=center>14.05</td><td align=center>37.40</td><td align=center>-</td>
    <td align=center>-</td><td align=center>-</td><td align=center>-</td>
		<td align=center>-</td><td align=center>-</td><td align=center>-</td>
    <td align=center>-</td><td align=center>-</td><td align=center>-</td>
	</tr>
  <tr>
		<td>TMN</td>
    <td align=center>-</td><td align=center>-</td><td align=center>-</td>
		<td align=center>-</td><td align=center>-</td><td align=center>-</td>
    <td align=center>22.92</td><td align=center>76.08</td><td align=center>35.17</td>
    <td align=center>-</td><td align=center>-</td><td align=center>-</td>
	</tr>
  <tr>
		<td>TGN</td>
    <td align=center><b>28.47</b></td><td align=center>44.20</td><td align=center>-</td>
		<td align=center>-</td><td align=center>-</td><td align=center>-</td>
    <td align=center><b>28.23</b></td><td align=center>76.26</td><td align=center><b>42.97</b></td>
    <td align=center><b>18.90</b></td><td align=center><b>31.02</b></td><td align=center>-</td>
	</tr>
  <tr>
		<td>QSPN</td>
    <td align=center>27.7</td><td align=center><b>59.2</b></td><td align=center><b>69.3</b></td>
		<td align=center>35.6</td><td align=center>79.4</td><td align=center><b>93.9</b></td>
    <td align=center>-</td><td align=center>-</td><td align=center>-</td>
    <td align=center>-</td><td align=center>-</td><td align=center>-</td>
	</tr>
  <tr>
    <td>MAN<br><I><b>(Only RGB)</b></I></td>
    <td align=center>-</td><td align=center>-</td><td align=center>-</td>
    <td align=center><b>46.53</b></td><td align=center><b>86.23</b></td><td align=center>-</td>
    <td align=center>27.02</td><td align=center><b>81.70</b></td><td align=center>41.16</td>
    <td align=center>-</td><td align=center>-</td><td align=center>-</td>
	</tr>
  <tr>
    <td>TGA<br><I><b>(weakly)</b></I></td>
    <td align=center>-</td><td align=center>-</td><td align=center>-</td>
    <td align=center>19.94</td><td align=center>65.52</td><td align=center>89.36</td>
    <td align=center>12.19</td><td align=center>39.74</td><td align=center>24.92</td>
    <td align=center>-</td><td align=center>-</td><td align=center>-</td>
	</tr>
</table>

*Note that highest priority IoU=0.5*

## Popular Implementations

### PyTorch

- None.

### TensorFlow

- [jiyanggao/TALL](<https://github.com/jiyanggao/TALL>)

### Others

- None.

## Licenses

[![CC0](http://i.creativecommons.org/p/zero/1.0/88x31.png)](http://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, [muketong](https://github.com/iworldtong) all copyright and related or neighboring rights to this work.

