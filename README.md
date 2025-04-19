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