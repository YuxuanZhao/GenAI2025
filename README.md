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
</table>

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
</table>

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
</table>

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
</table>

## 训练 LLM

<table>
  <tr>
    <td rowspan="10">Mulit_GPU</td>
    <td rowspan="4">DeepSpeed - Zero Redundancy Optimizer (ZERO)</td>
    <td>Zero-1: shard optimizer (Momentum, Variance)</td>
  </tr>
  <tr>
    <td>Zero-2: shard optimizer, gradient, FP32 parameters</td>
  </tr>
  <tr>
    <td>Zero-3: shard optimizer, gradient, FP32/ FP16 parameters</td>
  </tr>
  <tr>
    <td>GPU 可以使用 NVLink 来互传信息，有 900 GB/s，会有 overhead，但是不会太夸张</td>
  </tr>
  <tr>
    <td rowspan="2">Offload</td>
    <td>Optimizer Offload: GPU 只放 FP16 parameters，其他的全部放在 CPU</td>
  </tr>
  <tr>
    <td>Optimizer + model Offload: 所有的都放在 CPU，只给 GPU 传输需要的</td>
  </tr>
  <tr>
    <td rowspan="2">大语言模型到底有多大</td>
    <td>fp32: 8 * 10 ** 9 * 32 = 32 GB，使用 Adam optimizer 的话，Momentum 32 GB，Variance 32 GB</td>
  </tr>
  <tr>
    <td>fp16 (GPU): 8 * 10 ** 9 * 16 = 16 GB，gradient 和 weight 一样大：16 GB</td>
  </tr>
  <tr>
    <td>gradient accumulation</td>
    <td>mini batch 先不更新，直到做到我们想要的一个 batch 大小再更新，batch size 一般来说要足够大，一般一个 batch 里面需要有 40-60M token (DeepSeek V3, batch size 1920, 32k context)</td>
  </tr>
  <tr>
    <td>gradient checkpointing</td>
    <td>activation recomputation：只存必要的 important activation，需要用的时候再 recompute</td>
  </tr>

  <tr>
    <td rowspan="6">Long Context</td>
    <td rowspan="3">activation memory</td>
    <td>在做 back propagation 的时候，需要保存 attention weight 和 feed forward 的计算值，所以输入很长的时候（16k）训练需要非常大的空间</td>
  </tr>
  <tr>
    <td>參考 ultra scale playbook 或者 nvidia 在 2022 發表的 Reducing Activation Recomputation
in Large Transformer Models 的公式算法，GPT2 Like 一个 Transformer Block 的 Activation 需要 34sbh + 5as^2b bytes memory (s: tokens, b: batch, h: hidden, a: heads)，LLama3 8B hidden size: 4096, heads: 32，所以 Layer 需要 34sbh + 5as^2b = 34 * 16384 * 1 * 4096 + 5 * 32 * 16384^2 * 1 = 2.125 GB + 40GB</td>
  </tr>
  <tr>
    <td>Attention Activation 是 11sbh + 5as^2b，而 FeedFoward + Layer Norm 是 23sbh，所以更精确的 Attention 是 40.6875 GB 而 FeedForward + Layer Norm 是 1.4375 GB</td>
  </tr>
  <tr>
    <td rowspan="2">Flash Attention 3</td>
    <td>所需 vRAM 更少：因为优化了暂时不用的放在 CPU RAM，空间复杂度从 O(N**2) => O(N)</td>
  </tr>
  <tr>
    <td>training 更快：使用 Fused Kernel 把矩阵乘法，mask，softmax，dropout的操作全部压缩在一起</td>
  </tr>
  <tr>
    <td>Liger Kernel</td>
    <td>LLM 常用的操作 fuse 成一个优化后的 Triton 代码: 因为速度上 Pytorch < torch.compile() < Triton < CUDA</td>
  </tr>
  <tr>
    <td>Small vRAM inference</td>
    <td>Quantization</td>
    <td>Lossy compression can be 8B or less, even binary</td>
  </tr>
</table>

## Post-training

<table>
  <tr>
    <td rowspan="7">post-training</td>
    <td>pretrain/SFT</td>
    <td>foundation model 不一定是 pretrained model，也可以是 chat/instruct model。post-training 也不一定是 alignment</td>
  </tr>
  <tr>
    <td>catastrophic forgetting</td>
    <td>会遗忘 safety alignment 和其他的能力，和模型大小没什么关系</td>
  </tr>
  <tr>
    <td>LoRA</td>
    <td>遗忘得少了，但是学得也少，未来发展的方向会有更多不同专长的模型，比如说特别会编程的模型不会写唐诗是可以接受的，但是基本的沟通和模型间的指令应该要保持</td>
  </tr>
  <tr>
    <td>Experience Replay</td>
    <td>混一点 3-5% 之前的训练资料就好，没有训练资料的时候使用语言模型产生一点 QA 就好</td>
  </tr>
  <tr>
    <td>foundation model reparaphrase</td>
    <td>用 reparaphrase 数据来训练模型，效果会比直接用好</td>
  </tr>
  <tr>
    <td>self-output</td>
    <td>让模型/其他更好的模型产生/修改答案，如果答案是对的就拿来用（其实有点类似于 RL）</td>
  </tr>
  <tr>
    <td>放弃学不会的</td>
    <td>在 post-train 的时候如果某一个 token 模型很难预测出来（概率分布的偏差很大），直接放弃学这个 token，但是前后的那些 token 仍然正常学</td>
  </tr>
</table>

## Reasoning (test-time compute 的一种)

<table>
  <tr>
    <td rowspan="4">不用微调参数</td>
    <td>supervised CoT</td>
    <td>prompt 写得更长，更精确，先列出解题过程，再给出答案（few/ zero shot）</td>
  </tr>
  <tr>
    <td rowspan="3">给模型推理的工作流程</td>
    <td>parallel: 问同一个模型同一个问题，Large Language Monkeys</td>
  </tr>
  <tr>
    <td>sequential: 根据之前的解法再解一次，结合 parallel 和 sequential 可以得到一棵树，进而使用 monte carlo tree search/ beam search</td>
  </tr>
  <tr>
    <td>怎么知道哪个是正确的？<br>
    - Majority Vote (Self consistency) 非常强的 baseline<br>
    - Confidence<br>
    - Verifier: 是一个语言模型 (Best of N)，可以训练一下 input + ground truth<br>
    - Process verifier: 让模型解到底，然后看回来某个公共的 step 有多少几率能够得到正确最终结果，拟合这个几率</td>
  </tr>

  <tr>
    <td rowspan="10">需要微调参数</td>
    <td rowspan="4">教模型推理的过程 imitation learning</td>
    <td>SFT: 我们需要有 input + reasoning + ground truth 的数据：语言模型生成 reasoning</td>
  </tr>
  <tr>
    <td>RL: 没有 ground truth 甚至可以用语言模型生成 answer，用 verifier 检查</td>
  </tr>
  <tr>
    <td>journey learning > shortcut learning: 检查每个 step 是不是对的，但是这样不好，因为如果训练的时候没有见过错误的思考步骤，模型就不知道应该检查自己的思考过程</td>
  </tr>
  <tr>
    <td>knowledge distillation: 学习 reasoning model 的输出</td>
  </tr>

  <tr>
    <td rowspan="2">结果导向地学习推理 RL (Accuracy as reward)</td>
    <td>DeepSeek R1 Zero: 答案对就行，推理过程不重要，+Majority Vote 16 次就有非常好的效果</td>
  </tr>
  <tr>
    <td>但是 reasoning readability 非常差，因为只用了结果来衡量</td>
  </tr>

  <tr>
    <td rowspan="3">DeepSeek R1 训练过程</td>
    <td>DeepSeek v3 base =(Accuracy as reward)=> DeepSeek R1 Zero =(zero-/few- shot long CoT + human annotation)=> answers with reflection and verification</td>
  </tr>
  <tr>
    <td>DeepSeek v3 base =(Imitation Learning)=> model A =(RL Accuracy, language coherence)=> model B => generate many reasoning and answer for same question, verified by DeepSeek v3</td>
  </tr>
  <tr>
    <td>DeepSeek v3 base =(Imitation Learning)=> Model C =(RL Safety, Helpfulness)=> DeepSeek R1</td>
  </tr>
  <tr>
    <td>Foundation model 很重要</td>
    <td>因为 RL 是强化模型本身的能力，参数少本身能力有限的话，RL 远不如 Distill 有效（因为模型本身产不出正确答案的时候 RL 没用）</td>
  </tr>

  <tr>
    <td rowspan="3">Reasoning 长度要合适</td>
    <td>Chain of Draft</td>
    <td>对应 CoT，直接改变 prompt 要求模型输出的每一个 thinking step 都只是一个 5 字 draft</td>
  </tr>

  <tr>
    <td>给模型推理的工作流程 + imitation learning</td>
    <td>同一个输入，在模型所有的正确回答中选择 reasoning 过程最短的作为 training data</td>
  </tr>

  <tr>
    <td>max RL</td>
    <td>在原本 RL 的基础上，要求长度为 N，reasoning 过程小于等于 N 都可以接受，超过则有惩罚</td>
  </tr>

  <tr>
    <td rowspan="4">衡量 Reasoning 能力</td>
    <td>GSM8K</td>
    <td>记住了部分问题和答案，换一下人名，数字，reorder 句子，模型就不会了</td>
  </tr>

  <tr>
    <td>ARC-AGI</td>
    <td>找规律题，准确率改善最慢的标准，但是 o3 有突破</td>
  </tr>

  <tr>
    <td>Chatbot Arena</td>
    <td>人类对输出的风格是有倾向的(比如喜欢emoji)，用 Elo Score 来预计模型的战力，考虑模型实力以外的因素(Style and Sentiment Control)</td>
  </tr>

  <tr>
    <td>Goodhart's law</td>
    <td>一个指标一旦当成目标，这个指标的衡量效果就变坏了</td>
  </tr>
</table>

## Model Editing

<table>
  <tr>
    <td>对比 post training</td>
    <td>post training 学会的是新技能（语言，工具，推理）</td>
    <td>editing 植入一项知识（少量数据和改动），post training 做 model editing 容易学歪了</td>
  </tr>
  <tr>
    <td rowspan="3">目标</td>
    <td>Reliability</td>
    <td>目标要达成</td>
  </tr>
  <tr>
    <td>Generalization</td>
    <td>paraphrase, reverse, portability</td>
  </tr>
  <tr>
    <td>Locality</td>
    <td>不影响其他的问题的答案</td>
  </tr>
  <tr>
    <td>不改参数</td>
    <td>in context knowledge editing (IKE)</td>
    <td>直接提供的话模型不一定会采纳，可以用 one-shot/ few-shot，里面包含 reliability, generalization, locality 的例子</td>
  </tr>
  <tr>
    <td rowspan="4">改变参数</td>
    <td>人类决定: Rank-One Model Editing (ROME)</td>
    <td>找到神经网络中最相关的部分，修改这个部分的参数（不需要 gradient descent，是一个 closed-form solution）</td>
  </tr>
  <tr>
    <td rowspan="3">模型决定</td>
    <td>编辑模型(Hypernetwork)返回一个待编辑模型参数大小的结果，给待编辑模型的参数直接加上这个结果。这个思路是 Meta Learning，可以把这两个模型理解成合在一起，那这个结果其实是中间某一层的输出，冻住待编辑模型的参数，只训练编辑模型</td>
  </tr>
  <tr>
    <td>但是对于大型的语言模型来说这样的输出结果太大了不好训练，实际上paper里会设计一个 loss 和相应的 gradient descent，训练一个神经网络接受这个 gradient 输出需要编辑的参数变化</td>
  </tr>
  <tr>
    <td>但连这个神经网络都很难训练，参数量大到不可能。MEND 将 gradient 拆成 uv，这样的话神经网络只需要更新 uv 就好。但为什么 gradient 可以拆成 uv？有数学证明</td>
  </tr>
</table>