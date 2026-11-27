# 产出字段与核对规则

以 UTF-8 写入 CSV，使用标准 CSV 引用规则。用户已有字段能表达同一信息时，优先沿用并记录映射。适用时添加本轮所需的字段。

## 研究方案 `protocol.md`

写下研究问题、受众、目的、综述类型及理由、证据范围、纳排规则编号（如 I1、E1）、检索来源和限制、提取口径、复核方式及有日期的方案变更。仅限用户提供的材料时写明集合边界。

## 检索日志 `search_log.csv`

```csv
row_type,query_id,date,source,query,filters,sort,limit,query_status,failure_detail,rank,title,doi,source_url,record_id
```

- 每次尝试写一条 `query` 行，其状态为 `ok`、`partial`、`failed` 或 `supplied`。零命中的成功检索也写查询行。失败和部分返回注明原因。
- 每条实际纳入筛选范围的原始命中写一条 `hit` 行，通过 `query_id` 关联查询，保留原始排名及最终归入的 `record_id`。来源未提供排名时留空。重复命中保留各自的命中行，并指向同一篇文献编号。用户给定材料可以用一条 `supplied` 查询行关联每份实际收到的记录。
- 日期采用 YYYY-MM-DD，过滤条件包括实际使用的日期字段与范围。出处使用能够复查出版信息或全文的链接。

## 筛选表 `eligibility.csv`

```csv
record_id,title,authors,year,doi,source_url,publication_type,screening_stage,decision,rule_ids,reason,verification_source,reviewer
```

每篇去重后文献一行，终态只能是 `include`、`exclude`、`pending` 之一。复核依据用来源链接加所在位置。待核行写明缺失项及下一步可核对的线索。跨阶段的判断历史如需保留，另设记录字段，统计只读当前决定。

## 证据表 `evidence_matrix.csv`

```csv
record_id,citation,doi,full_text_url,research_task,population,sample,design,intervention,comparator,outcome_and_assessor,reported_result,source_locator,limitations,review_interpretation
```

每篇纳入文献一行。多项结果在指标、结果和原文位置三列使用同样的编号对应，例如 `[1] 内容评分／独立评分者`、`[1] 第 4 表`。结果写上适用的统计量、单位或分母及比较条件。原文缺稳定页码时，用章节标题加表号、图号或可查找的段落锚点。

## 综述 `test_report.md`

推荐顺序为研究问题与证据范围、检索筛选方法及数量、按问题组织的发现、局限和参考文献。正文使用 `[P001]` 等编号指向证据表，编号后的参考文献条目提供 DOI 或稳定链接。

## 交付核对

1. 以命中行数为检索记录数，以筛选表的唯一编号数为去重后记录数。差值应等于合并的重复命中数。查询行与失败次数单独报告。确认每条命中的编号都对应筛选行，每个筛选编号至少有一条命中。
2. 核对纳入数 + 排除数 + 待核数 = 去重后记录数。证据表编号集合与筛选表中纳入编号集合应完全一致，每个编号只出现一次。
3. 抽查正文至少一条关键判断，由文献编号回到证据表，再核对原文的比较对象、数字、单位和位置。覆盖多条关键结论时，逐条检查它们所引用的证据。
4. 核对正文的覆盖和复核陈述与实际执行一致。按排名限量、部分数据库访问失败、仅一轮筛选或来源不足时，写清对结论的影响。已核实的纳入量为零时，保留空证据表的表头，并报告目前可确认的范围及待核情况。
