##### 可得时间对齐与有序合并

行情更新表示为“发布时间、资产、数值”，决策为“决策时间、资产”，两个列表各自按时间升序。返回每次决策前已到达的最近一条同资产记录，记录年龄超过容忍间隔则返回 None。这里按同一时间戳内先决策、后到达更新的约定处理，因此同刻更新不能使用。对于同资产、同发布时间的多条更新，约定输入中靠后的一条覆盖之前的值

```python
def latest_known(decisions, releases, max_age):
    latest = {}
    i = 0
    output = []
    for when, asset in decisions:
        while i < len(releases) and releases[i][0] < when:
            published, name, value = releases[i]
            latest[name] = (published, value)
            i += 1
        row = latest.get(asset)
        if row is None or when - row[0] > max_age:
            output.append(None)
        else:
            output.append(row[1])
    return output


updates = [(1, "A", 10), (2, "B", 20), (3, "A", 12), (3, "A", 13)]
queries = [(1, "A"), (2, "A"), (3, "A"), (3, "B"), (4, "A"), (7, "A")]
assert latest_known(queries, updates, 2) == [None, 10, 10, 20, 13, None]
assert latest_known([(5, "X")], [], 4) == [None]
```

决定时点 3 的 A 只能用时点 1 的 10，到时点 4 才能用同刻两条修订中靠后的 13。每条更新最多读一次，$n$ 次决策与 $m$ 条更新共 $O(n+m)$ 时间，除返回结果外，状态最多占用 $O(A)$ 空间。若输入无序，先排序并保留查询原有行号；若数据以报告期和公告日双标时间，应使用真实可见的公告时间对齐

##### 时间键与等时事件

同样的规则对应 pandas.merge_asof 的向后搜索、按资产匹配、设定 tolerance、关闭 allow_exact_matches，两侧都须按时间键全局递增。允许同刻成交究竟读取同刻公告，取决于可验证的消息顺序；时间分辨率不足时，严格早于决策时刻是一种可复现的边界定义。若使用 nearest，可能拿到未来一条更近的记录。[pandas.merge_asof 参数及排序要求](https://pandas.pydata.org/docs/reference/api/pandas.merge_asof.html)
