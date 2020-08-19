# 高效的关键词替换和敏感词过滤工具

## 1. 算法介绍

利用高效的Trie树建立关键词树，如下图所示，然后依次查找字符串中的相连字符是否形成树的一条路径

<img src="images/trie.png" alt="trie" style="zoom:33%;" />

发现掘金上[这篇文章](https://juejin.im/post/6844903750490914829)写的比较详细，可以一读，具体原理在此不详述。

## 2. 关键词替换

```go
replacer := stringx.NewReplacer(map[string]string{
  "PHP": "PPT",
  "世界上": "吹牛",
})
fmt.Println(replacer.Replace("PHP是世界上最好的语言！"))
```

可以得到：
```
PPT是吹牛最好的语言！
```

示例代码见`example/stringx/replace/replace.go`

## 3. 敏感词过滤

```go
filter := stringx.NewTrie([]string{
  "AV演员",
  "苍井空",
  "AV",
  "日本AV女优",
  "AV演员色情",
}, stringx.WithMask('?'))
safe, keywords, found := filter.Filter("日本AV演员兼电视、电影演员。苍井空AV女优是xx出道, 日本AV女优们最精彩的表演是AV演员色情表演")
fmt.Println(safe)
fmt.Println(keywords)
fmt.Println(found)
```

可以得到：

```
日本????兼电视、电影演员。?????女优是xx出道, ??????们最精彩的表演是??????表演
[苍井空 日本AV女优 AV演员色情 AV AV演员]
true
```

示例代码见`example/stringx/filter/filter.go`

## 4. Benchmark

| Sentences | Keywords | Regex    | Go-Zero  |
|-----------|----------|----------|----------|
| 10000     | 10000    | 16min10s | 27.2ms   |
