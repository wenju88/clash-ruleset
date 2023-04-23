# 自定义 rule-set
# 一、 说明
1. 每天早上 3 点（北京时间）自动构建生成 advertising.yaml、tracker.yaml、networktest.yaml、google-cn.yaml 和 user.yaml
2. advertising.yaml 源采用 [blackmatrix7/ios_rule_script/AdvertisingLite](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Clash/AdvertisingLite)、[blackmatrix7/ios_rule_script/Hijacking](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Clash/Hijacking) 和 [blackmatrix7/ios_rule_script/Privacy](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Clash/Privacy) 组合
3. tracker.yaml 源采用 [TrackersList](https://trackerslist.com)（all.txt）
4. networktest.yaml 源采用 [blackmatrix7/ios_rule_script/Speedtest](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Clash/Speedtest) 和 [IPv6 测试网站](https://github.com/DustinWin/clash-geosite/blob/master/Rule-Files/network.txt)组合
5. google-cn.yaml 源采用 [rules.kr328.app/google@cn](https://rules.kr328.app/google@cn.yaml)（删除 `'+.googleapis.cn'`，以免直连时出现 [Google Play Store](https://play.google.com/store) 无法下载或升级应用的问题）
6. user.yaml  
① 若想自己生成配置文件 user.yaml，可以 [Fork 本项目](https://github.com/DustinWin/clash-ruleset/fork)后编辑 *.github/workflows/run.yml* 内的 `name: Put together user.yaml` 部分和 *UserConfig* 目录下的.yaml 文件  
② 编辑 *MyConfig/later-user.yaml* 文件，将 `nameserver` 中的`🪜 代理域名`改成可以访问外网的代理组名，或者直接将 `'https://dns.google/dns-query#🪜 代理域名'`修改为 `tls://dns.google`  
③ 添加 [NTP 服务](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Clash/NTPService)到 user.yaml 内的 `fake-ip-filter` 中，防止校时失败  
④ 添加 [TrackersList](https://trackerslist.com) 到 user.yaml 内的 `fake-ip-filter` 中，防止 [BT 下载](https://github.com/c0re100/qBittorrent-Enhanced-Edition)无法连接 TrackersList UDP 协议
# 二、 使用方法
## 1. rule-set
在配置文件中新增如下内容：
- 注：以下只是节选，请酌情套用

```
proxy-groups:
  - name: 📈 网络测试
    type: select
    proxies:
      - 🎯 全球直连
      - 🚀 节点选择

  - name: 🗽 Google 中国
    type: select
    proxies:
      - 🎯 全球直连
      - 🚀 节点选择

  - name: ⛓️ BT 下载
    type: select
    proxies:
      - 🎯 全球直连

  - name: ⛔️ 广告域名
    type: select
    proxies:
      - 🛑 全球拦截

  - name: 🎯 全球直连
    type: select
    proxies:
      - DIRECT

  - name: 🛑 全球拦截
    type: select
    proxies:
      - REJECT

rule-providers:
  advertising:
    type: http
    behavior: classical
    url: "https://fastly.jsdelivr.net/gh/DustinWin/clash-ruleset@release/advertising.yaml"
    path: ./ruleset/advertising.yaml
    interval: 86400

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

rules:
  - RULE-SET,advertising,⛔️ 广告域名
  - RULE-SET,tracker,⛓️ BT 下载
  - RULE-SET,networktest,📈 网络测试
  - RULE-SET,google-cn,🗽 Google 中国
```
## 2. user.yaml
① 导入 ShellClash  
连接 SSH 后执行如下命令：
```
curl -o $clashdir/user.yaml -L https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/user.yaml
```
② 导入 Clash Verge（Windows 端）  
首次使用可进入“配置”，新建”Merge“类型的配置，保存后进入文件夹 *%USERPROFILE%.config\clash-verge\profiles*，可以看到这里新增了一个.yaml 文件，复制其文件名并替换下面命令中的{文件名}  
以管理员身份打开 CMD 命令提示符，执行如下命令：
```
curl -o %USERPROFILE%\.config\clash-verge\profiles\{文件名}.yaml -L https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/user.yaml
```
