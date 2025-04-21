# GenAI

| Action | 通用 ML | Year |
| --- | ---------------- | ---- |
| train downstream | BERT | 2018-2019 |
| Finetune | GPT-3 | 2020-2022 |
| Prompt | ChatGPT | 2023-present |

## AI agent

<table>
  <tr>
    <td>AI agent</td>
    <td>Alpha Go, Operator, real-time interaction</td>
    <td>goal => observation => action => observation => action ...</td>
  </tr>

  <tr>
    <td rowspan="6">根据经验</td>
    <td>不是改参数，而是看历史</td>
    <td>但是 agent 可能会有非常多的 action 和 observation，全部看反而不好</td>
  </tr>
  <tr>
    <td>Read</td>
    <td>AI agent 需要 memory，使用 retrieval 来筛选现在重要的信息（可以用 RAG 相似的方法）</td>
  </tr>
  <tr>
    <td>Write</td>
    <td>如果所有的 observation 都放到 memory，会充斥着鸡毛蒜皮的事，筛选后放入</td>
  </tr>
  <tr>
    <td>Reflect</td>
    <td>整理 memory 到更抽象，比如说 knowledge graph (GraphRAG, HippoRAG)</td>
  </tr>
  <tr>
    <td>Benchmark & 相关文献</td>
    <td>StreamBench & MemGPT, Agent Workflow Memory, Agentic Memory for LLM Agents</td>
  </tr>
  <tr>
    <td>观察</td>
    <td>负面的反馈只有副作用，只放正面例子会更有效</td>
  </tr>

  <tr>
    <td rowspan="7">使用工具</td>
    <td>function call</td>
    <td>搜索引擎，代码能力，声音图片处理，其他AI等</td>
  </tr>
  <tr>
    <td>平衡</td>
    <td>语言模型很简单就能完成的任务就没必要用工具</td>
  </tr>
  <tr>
    <td>system prompt</td>
    <td>告诉模型如何使用工具，以及有哪些工具</td>
  </tr>
  <tr>
    <td>工具太多</td>
    <td>system prompt 也放到 memory 里面，然后筛选的时候同时筛选有用的 tool</td>
  </tr>
  <tr>
    <td rowspan="3">说服模型</td>
    <td>外部知识和AI内部的信息差异越小</td>
  </tr>
  <tr>
    <td>AI写的文章会比较容易说服其他 AI</td>
  </tr>
  <tr>
    <td>Metadata: 发表时间越晚，AI 越相信，来源没有影响，呈现的方式（好看的网页）</td>
  </tr>

  <tr>
    <td rowspan="4">做计划</td>
    <td>Benchmark</td>
    <td>PlanBench, Mystery Block World, Travel Planner 计划最好可以灵活变化</td>
  </tr>
  <tr>
    <td>Tree search</td>
    <td>暴搜实际试试，路径太长的时候剪枝没希望的</td>
  </tr>
  <tr>
    <td>覆水难收</td>
    <td>脑内小剧场，不要在现实中执行，所以需要 World Model（AI 自己扮演的话就是 reasoning）</td>
  </tr>
  <tr>
    <td>danger of overthinking</td>
    <td>实际尝试的代价可能很低，这个时候其实不需要想那么久</td>
  </tr>
<table>

## How LLM works?

<table>
  <tr>
    <td rowspan="6">一个神经元</td>
    <td>数值</td>
    <td>一个token对应的embedding的向量，经过FC和relu之后的其中一个数值（很大的值代表被启动）</td>
  </tr>
  <tr>
    <td>相关性不是因果性</td>
    <td>移掉就没有某种表现就证明有因果性（永远设成0或者平均值）</td>
  </tr>
  <tr>
    <td>程度</td>
    <td>不同启动的等级会不会体现在行为上（更脏的脏话），很少研究这个因为很难定义什么是更脏的脏话</td>
  </tr>
  <tr>
    <td>一件事由很多神经元共同管理</td>
    <td>不容易解释单一神经元的功能（单复数神经元也只是有影响，但不是完全控制）</td>
  </tr>
  <tr>
    <td>单一神经元可能管很多事</td>
    <td>某种神经元排列控制一件事</td>
  </tr>
  <tr>
    <td>为什么不是1:1的？</td>
    <td>因为这样能干的事就太少了，参数量不够</td>
  </tr>

  <tr>
    <td rowspan="8">一层网络</td>
    <td>行为</td>
    <td>每个layer都有一层self attention layer，之后这个sequence的每个token的embedding变成一个向量，每个向量经过很多个MLP输出最后才得到结果</td>
  </tr>
  <tr>
    <td rowspan="4">representation engineering, activation engineering, activation steering</td>
    <td>抽取拒绝向量：观察到的向量 = 拒绝向量 + 其他向量的平均，找到没有拒绝的其他向量的平均，就可以算出来拒绝向量</td>
  </tr>
  <tr>
    <td>验证拒绝向量：在正常问题中加上这个拒绝向量，看看会不会拒绝。在需要拒绝的问题中减去拒绝向量，看看会不会通过</td>
  </tr>
  <tr>
    <td>in-context vector: context是同义词，反义词，首字母等。把例子的最后输出平均值当作 in-context vector，接到 zero-shot 问题后面模型就会做出相同的处理</td>
  </tr>
  <tr>
    <td>其他例子：sycophancy vector 谄媚向量，truthful vector 说真话向量</td>
  </tr>
  <tr>
    <td rowspan="3">Sparse Auto Encoder</td>
    <td>找出来某一层所有的功能向量：假设向量是k个功能向量的加权和</td>
  </tr>
  <tr>
    <td>一个不是这些功能向量的其他向量应该尽可能的小。同时为避免模型找到一个one hot trivial solution，所有的权重越小越好</td>
  </tr>
  <tr>
    <td>Claude 3 Sonnet 找了三千四百万个功能向量：debug, 金门大桥, AI 觉得自己是 AI，sycophancy</td>
  </tr>

  <tr>
    <td rowspan="4">一群神经元</td>
    <td>GPT 整个网络</td>
    <td>输入一个 token 序列，转换成 embedding，通过 layer 变成另外的 embedding，类似的 layer 很多层之后最后一个向量做 unembedding，输出 token 的概率分布</td>
  </tr>
  <tr>
    <td rowspan="3">Circuit</td>
    <td>语言模型的模型，需要 faithfulness，也就是尽可能类似于原本的语言模型，但是要更小更简单，来更容易理解完成某个任务时的背后机制是什么</td>
  </tr>
  <tr>
    <td>这样有用吗？可以用这个简单模型帮助模型编辑，也就是找到给 embedding 加上 delta x 来达到我们想要的输出</td>
  </tr>
  <tr>
    <td>怎么找到这个模型呢？大量的 pruning 直到一目了然，只需要保持关心的能力没有改变（答案还是保持不变的）</td>
  </tr>

  <tr>
    <td rowspan="5">模型解释自己的想法</td>
    <td>high level</td>
    <td>直接问模型为什么这么想</td>
  </tr>
  <tr>
    <td>logic lens</td>
    <td>在中间的层过 softmax 之前做 unembedding 得到 distribution。llama2 的 inner speech 是英文，输入输出都会先转换成英文进行思考</td>
  </tr>
  <tr>
    <td>Patchscopes</td>
    <td>logic lens 只能是一个 token 的 representation，也就是预测下一个 token。用对应关系来看输入的内容在模型的理解是什么</td>
  </tr>
  <tr>
    <td>Back Patching</td>
    <td>类似于 reasoning 的想法，嵌套的逻辑关系如果来不及在一次 feed forward 里得到清楚，那就把后层输出的结果直接丢回到前层</td>
  </tr>
  <tr>
    <td>MLP = Key-value memories</td>
    <td>每一个 layer 其实就是往 residual stream 里加了点什么，那加的是什么呢？我们可以把 MLP 看作是 key-value memories，也就是 attention。我们可以找到某一层的输入的其中一个神经元的v，减去正确答案的 token embedding，再加上想要的答案的 token embedding，就有机会改变答案成想要的</td>
  </tr>
<table>

## Mamba

<table>
  <tr>
    <td rowspan="4">RNN-style</td>
    <td>Hidden state</td>
    <td>决定下一个时刻的结果的全部信息</td>
  </tr>
  <tr>
    <td>LSTM</td>
    <td>决定上一时刻的 hidden state (forget gate)，这一时刻的输入 x (input gate) 和这一时刻的输出(output gate) 都和当前的时刻有关，也就是 3 个 gate</td>
  </tr>
  <tr>
    <td>线性</td>
    <td>不能平行运算，因为必须计算之前所有的 hidden state 才能算，需要等的话 GPU 的效果就很差，但是计算量和memory需求比较固定</td>
  </tr>
  <tr>
    <td>类比大模型 memory</td>
    <td>初始的 hidden state 相当于 memory，input gate 是 write，output gate 是 read，forget gate 是 reflect</td>
  </tr>

  <tr>
    <td rowspan="4">Self Attention</td>
    <td>过程</td>
    <td>输入x进来，变换成qkv，然后q和k做attention score，softmax 之后加权和v</td>
  </tr>
  <tr>
    <td>attention is all you need paper</td>
    <td>证明拿掉所有其他的东西效果还很好</td>
  </tr>
  <tr>
    <td>缺点</td>
    <td>attention 的问题在于序列越长，计算越困难，memory也占用更大</td>
  </tr>
  <tr>
    <td>优点</td>
    <td>训练的时候更好地平行化使用 GPU 的性能，给完整输入，生成每个 token 是平行的</td>
  </tr>

  <tr>
    <td rowspan="11">平行化 RNN</td>
    <td>Mamba v1</td>
    <td>不要 forget gate 了，直接把上一时刻的 hidden state 拿过来，可以用 scan algorithm 的方法来加速</td>
  </tr>
  <tr>
    <td rowspan="3">Linear Attention</td>
    <td>如果reflect 这一步用的矩阵可以用 D = vk 来表示，那么我们发现这个简化后的 RNN 现在就类似于 self attention 没有 softmax</td>
  </tr>
  <tr>
    <td>v 和 k 是什么，v 是要写入 memory 的内容，k 决定要写进哪里，q 决定从哪个 column 取出多少的信息</td>
  </tr>
  <tr>
    <td>训练的时候像 self attention，inference 的时候像 RNN（更快）</td>
  </tr>

  <tr>
    <td rowspan="2">为什么 linear attention 效果差于 self attention?</td>
    <td>不是因为 linear attention 只有有限的记忆，因为 transformer 也有问题。如果 query 的长度（一般是4096）小于序列的长度（一般远超 4096），那么 query 不想要的信息也会被拿到，因为 d 维空间里只能有 d 个垂直的向量，softmax 之后就会把这些不想要的地方也加权和了</td>
  </tr>
  <tr>
    <td>差在 softmax，linear attention 缺失了 forget gate 就永远不会遗忘了。softmax 使得模型能够保存更重要的事情，因为重要性是对比出来的，本来重要的事情在更重要的事情出现之后就不重要了</td>
  </tr>

  <tr>
    <td rowspan="5">改进</td>
    <td>retention network：乘上一个 gamma 来做遗忘</td>
  </tr>
  <tr>
    <td>gated retention (Mamba 2): 不是每一件事都均匀随着时间遗忘，而是用sigmoid(Wx)</td>
  </tr>
  <tr>
    <td>更复杂的 reflection：elementwise 乘上 hidden state</td>
  </tr>
  <tr>
    <td>DeltaNet：在放新的信息之前，先清除掉一部分之前的信息，这样可能更好地记住当前的信息</td>
  </tr>
  <tr>
    <td>Titans (Learning to Memorize at Test Time): hidden state 一般被认为是 memory，但是这里的操作实际上类似于做 gradient descent，hidden state 可能是一种特殊的 parameter？loss 这个时候在优化的是用 kt 取资料的时候，取出来的东西和 vt 越接近越好</td>
  </tr>
<table>

## Pretrain & Alignment/Finetune (SFT & RLHF)

<table>
  <tr>
    <td rowspan="5">Pretrain</td>
    <td rowspan="3">挑数据</td>
    <td>rephrase web 更适合训练模型 (Apple)，一定要去重，尽量看不同的资料，同样的资料很快就会 converge（超过4次就有明显区别）</td>
  </tr>
  <tr>
    <td>pretrain 的时候如果一个人只有一个描述，那么模型很有可能会有误解。如果类似的描述有多个，模型就会搞清楚（需要 second opinion）</td>
  </tr>
  <tr>
    <td>而且不需要每个人的描述都有多个，只需要有一些是这样的就行。模型就明白原来要这样理解！
  </td>
  </tr>
  <tr>
    <td>数据量</td>
    <td>资料越多越好：LLAMA3，Deepseek v3 15T token = FineWeb HuggingFace</td>
  </tr>
  <tr>
    <td>数据清洗的重要性</td>
    <td>shift cipher 1, 3, 13 encode 在 pretrain data 里都有比较多，pretrain 里学了很多不该学的，alignment 也只能压抑输入的激发，那些脏话还是存在于neuron中难以清除</td>
  </tr>
  <tr>
    <td rowspan="8">Alignment</td>
    <td rowspan="2">挑数据</td>
    <td>质量的定义：越长越好，用好模型回答是最有效的 (相当于distillation，弱智吧都行)</td>
  </tr>
  <tr>
    <td>Alignment 的内容应当是模型本来知道的知识，使用模型不知道的内容进行 finetune 会立刻开始 overfit，能力下降。Maybe known 的资料是最有用的（要用正确问法才能问到正确答案），不要在 alignment 的时候还去教模型新的信息</td>
  </tr>
  <tr>
    <td>数据量</td>
    <td>很少的数据，画龙点睛。为啥呢？其实模型的行为在 alignment 之后的差异很小（一步错步步错，不会停）</td>
  </tr>
  <tr>
    <td>挑问题</td>
    <td>问题不重要，不是问题也没事，没有问题也没事</td>
  </tr>
  <tr>
    <td>不 finetune，直接修改</td>
    <td>增加结束符号的几率，更改某些符号的出现几率，避免出现重复内容</td>
  </tr>
  <tr>
    <td>Self-rewarding language Models</td>
    <td>让模型给自己的同一问题的不同回答打分</td>
  </tr>
  <tr>
    <td>极限</td>
    <td>finetune 看起来像模像样，但是实际上是错的内容</td>
  </tr>
  <tr>
    <td>RLHF</td>
    <td>因为逼迫模型做它不想做的事情是很难的(SFT)，但是它做得好的时候给它奖励(RL)就比较可行</td>
  </tr>
<table>