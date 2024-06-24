# Inverted File Index

## Definition

倒排索引中，对于每个单词，都记录了包含这个单词的文档数和指向这些文档的指针。

<div align="center"><img src="/assets/img/cs/ads/chapter3/inverted-file-index.png" width="70%"></div>

对于每个单词出现的每个文档，可以记录出现的次数和位置：

<div align="center"><img src="/assets/img/cs/ads/chapter3/inverted-file-index-detail.png" width="70%"></div>

## Optimization

### Word Stemming

词干提取，将单词转换为其词干形式，如将 "running" 转换为 "run"。

### Stop Words

停用词，如 "a", "the", "is" 等，不记录这些单词。

### Access Documents

存储倒排索引的数据结构：

1. 搜索树：B+、B-、Trie 等
2. 哈希表

### Distributed Index

- Term Partitioning：将不同段的倒排索引分别存储在不同的磁盘上
- Document Partitioning：将不同文档的倒排索引分别存储在不同的磁盘上

### Not Enough Memory

当内存不足时，将当前内存中的倒排索引写入磁盘，再读取下一部分。

当磁盘空间不足时，可以将倒排索引分为多个部分，分别存储在不同的磁盘上。一般采用分布式存储，查询时调度多个磁盘并行读取。

### Dynamic Index

建立主表和辅助表，辅助表动态更新，存储新插入的文档。查询时在主表和辅助表中查找。

删除时，对删除的文档进行标记，不用立即修改倒排索引。

### Compression

对倒排索引进行压缩，只存储 index 的差值。

### Thresholding

检索时，计算每个文档的权重，只检索权重在前 \(x\) 的文档。

查询时，将查询词出现的次数视为权重，根据权重高的查询词优先检索。

### Measures of the Search Engine

分为以下指标：

- 检索速度（index）：检索到文档的数量/时间
- 搜索速度（search）：以检索大小为自变量的延迟
- Expressiveness of query language：查询语言的表达能力

    - 复杂查询需求
    - 复杂查询的速度

不同的评判类别：

- Data retrieval 数据检索：主要看响应时间和检索大小
- Information retrieval 信息检索：主要看检索结果的准确性

### Relevence Measurement

计算查询词与文档的相关性，

<div align="center"><img src="/assets/img/cs/ads/chapter3/relevent.png" width="30%"></div>

<div align="center">
    <table>
        <tr>
            <th></th>
            <th>Relevent</th>
            <th>Irrelevent</th>
        </tr>
        <tr>
            <td>Retrieved</td>
            <td><math xmlns="http://www.w3.org/1998/Math/MathML"><msub><mi>R</mi><mn>R</mn></msub></math></td>
            <td><math xmlns="http://www.w3.org/1998/Math/MathML"><msub><mi>I</mi><mn>R</mn></msub></math></td>
        </tr>
        <tr>
            <td>Not Retrieved</td>
            <td><math xmlns="http://www.w3.org/1998/Math/MathML"><msub><mi>R</mi><mn>I</mn></msub></math></td>
            <td><math xmlns="http://www.w3.org/1998/Math/MathML"><msub><mi>I</mi><mn>I</mn></msub></math></td>
        </tr>
    </table>
</div>

- 准确率（Precision）：\(P = \frac{R_R}{R_R + I_R}\)

    衡量检索结果中相关文档的比例。

- 召回率（Recall）：\(R = \frac{R_R}{R_R + R_I}\)

    衡量相关文档中被检索到的比例。

<div align="center"><img src="/assets/img/cs/ads/chapter3/precision-recall.png" width="60%"></div>