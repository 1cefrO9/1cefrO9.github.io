+++
date = '2025-04-25T16:53:23+08:00'
title = 'My First Post'
categories = ["通用技术"]
tags = ["博客搭建", "Bilibili"]
+++

这是第一篇文章
行内数学公式：$a^2 + b^2 = c^2$。

块公式，

$$
a^2 + b^2 = c^2
$$
```html
<div>
$$
\boldsymbol{x}_{i+1}+\boldsymbol{x}_{i+2}=\boldsymbol{x}_{i+3}
$$
</div>
```

```css
.post-content pre,
code {
  font-family: "JetBrains Mono", monospace;
  font-size: 1rem;
  line-height: 1.2;
}
```

```css
baseURL: "https://sonnycalcr.github.io/" # 主站的 URL
title: SonnyCalcr's Blog # 站点标题
copyright: "[©2024 SonnyCalcr's Blog](https://sonnycalcr.github.io/)" # 网站的版权声明，通常显示在页脚
theme: PaperMod # 主题
languageCode: zh-cn # 语言

enableInlineShortcodes: true # shortcode，类似于模板变量，可以在写 markdown 的时候便捷地插入，官方文档中有一个视频讲的很通俗
hasCJKLanguage: true # 是否有 CJK 的字符
enableRobotsTXT: true # 允许生成 robots.txt
buildDrafts: false # 构建时是否包括草稿
buildFuture: false # 构建未来发布的内容
buildExpired: false # 构建过期的内容
enableEmoji: true # 允许 emoji
pygmentsUseClasses: true
defaultContentLanguage: zh # 顶部首先展示的语言界面
defaultContentLanguageInSubdir: false # 是否要在地址栏加上默认的语言代码



languages:
  zh:
    languageName: "中文" # 展示的语言名
    weight: 1 # 权重
    taxonomies: # 分类系统
      category: categories
      tag: tags
    # https://gohugo.io/content-management/menus/#define-in-site-configuration
    menus:
      main:
        - name: 首页
          pageRef: /
          weight: 4 # 控制在页面上展示的前后顺序
        - name: 归档
          pageRef: archives/
          weight: 5
        - name: 分类
          pageRef: categories/
          weight: 10
        - name: 标签
          pageRef: tags/
          weight: 10
        - name: 搜索
          pageRef: search/
          weight: 20
        - name: 关于
          pageRef: about/
          weight: 21

# https://github.com/adityatelange/hugo-PaperMod/wiki/Features#search-page
outputs:
  home:
    - HTML # 生成的静态页面
    - RSS # 这个其实无所谓
    - JSON # necessary for search, 这里的配置修改好之后，一定要重新生成一下

# 搜索
fuseOpts:
    isCaseSensitive: false # 是否大小写敏感
    shouldSort: true # 是否排序
    location: 0
    distance: 1000
    threshold: 0.4
    minMatchCharLength: 0
    # limit: 10 # refer: https://www.fusejs.io/api/methods.html#search
    keys: ["title", "permalink", "summary", "content"]
    includeMatches: true

# 评论的设置
giscus:
    repo: "1cefrO9/1cefrO9.github.io"
    repoId: "R_kgDOOfgFrw"
    category: "Announcements"
    categoryId: "DIC_kwDOOfgFr84CpdEI"
    mapping: "pathname"
    strict: "0"
    reactionsEnabled: "1"
    emitMetadata: "0"
    inputPosition: "bottom"
    lightTheme: "light"
    darkTheme: "dark"
    lang: "zh-CN"
    crossorigin: "anonymous"
```

```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

// 构建KMP算法的next数组
vector<int> buildNext(const string& pattern) {
    int n = pattern.size();
    vector<int> next(n, 0);    // next数组初始化为0
    int j = 0;                 // 前缀指针

    for (int i = 1; i < n; i++) { // 后缀指针从1开始
        while (j > 0 && pattern[i] != pattern[j]) {
            j = next[j - 1];      // 回溯到前一个匹配位置
        }
        if (pattern[i] == pattern[j]) {
            j++;                  // 匹配成功，前后缀指针同时后移
        }
        next[i] = j;              // 记录当前位置的最长公共前后缀长度
    }
    return next;
}

// KMP主匹配函数
vector<int> kmpSearch(const string& text, const string& pattern) {
    vector<int> matches;          // 存储所有匹配的起始位置
    if (pattern.empty()) return matches;

    vector<int> next = buildNext(pattern);
    int j = 0;                    // 模式串指针
    int m = text.size(), n = pattern.size();

    for (int i = 0; i < m; i++) { // 遍历文本串
        while (j > 0 && text[i] != pattern[j]) {
            j = next[j - 1];      // 利用next数组跳过不必要的比较
        }
        if (text[i] == pattern[j]) {
            j++;
        }
        if (j == n) {             // 完全匹配成功
            matches.push_back(i - n + 1);
            j = next[j - 1];       // 继续寻找下一个可能的匹配
        }
    }
    return matches;
}

int main() {
    string text = "ABABDABACDABABCABAB";
    string pattern = "ABABCABAB";
    
    vector<int> result = kmpSearch(text, pattern);
    
    if (result.empty()) {
        cout << "Pattern not found in text." << endl;
    } else {
        cout << "Pattern found at positions: ";
        for (int pos : result) {
            cout << pos << " ";
        }
        cout << endl;
    }
    return 0;
}
```