# 自定义 rule-set
# 一、 说明
1. 每天早上 3 点（北京时间）自动构建生成 tracker.yaml、networktest.yaml 和 google-cn.yaml
2. tracker.yaml 源采用 [TrackersList](https://trackerslist.com)（all.txt）
3. networktest.yaml 源采用 [blackmatrix7/ios_rule_script/Speedtest](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Clash/Speedtest) 和 [IPv6 测试网站](https://github.com/DustinWin/clash-geosite/blob/master/Rule-Files/network.txt)组合
4. google-cn.yaml 源采用 [rules.kr328.app/google@cn](https://rules.kr328.app/google@cn.yaml)（删除 `'+.googleapis.cn'`，以免直连时出现 [Google Play Store](https://play.google.com/store) 无法下载或升级应用的问题）
# 二、 使用方法
在 `rule-providers` 中添加如下内容：
```
rule-providers:
  tracker:
    type: http
    behavior: classical
    url: "https://fastly.jsdelivr.net/gh/DustinWin/clash-ruleset@release/tracker.yaml"
    path: ./ruleset/tracker.yaml
    interval: 86400

  networktest:
    type: http
    behavior: classical
    url: "https://fastly.jsdelivr.net/gh/DustinWin/clash-ruleset@release/networktest.yaml"
    path: ./ruleset/networktest.yaml
    interval: 86400

  google-cn:
    type: http
    behavior: domain
    url: "https://fastly.jsdelivr.net/gh/DustinWin/clash-ruleset@release/google-cn.yaml"
    path: ./ruleset/google-cn.yaml
    interval: 86400
```
