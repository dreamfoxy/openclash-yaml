openwrt 软路由openclash 精细分流ini 模版
发表评论 / 插件分享, 文章, 软件分享, 魔珐vÞņ翻越 / 作者：xiaoji
￼
￼
￼
￼
实用小技的照片
这可能是最干净实用的一套OpenClash分流模版！国外分流精准精细，有效防止dns泄漏，访问国内网站跟没开vpn的感觉一模一样，保姆级教你详细配置翻墙必备，这是一套可以“长期使用” 的机场订阅方案这可能是最干净实用的一套OpenClash分流模版！国…
￼
稍后观看
￼
分享
￼
￼
￼
如果你的openclash 用了几年，规则越加越多，节点越选越乱，把 DNS 交给系统层去“碰运气”，更新一次订阅，直接崩掉，要么分流失效，网络时好时坏，想使用homeproxy 分流又无法使用新版内核，还经常配置失败，dns泄漏问题严重。
今天给大家分享的这套分流规则订阅模版，是真正可以长期使用，不会越用越乱的一套流架构，结构清晰，使用简单，还可以增加自己的应用分流，真正做到精准精细分流，多次兜底，不会dns泄漏。
1、先在系统层绕掉 80% 不需要代理的流量
（私网 / 中国 IP / 本地服务，根本不进 OpenClash）
剩余遗漏的中国ip，再次通过配置私网+中国域名中国ip+补丁等规则集判断直连兜底，这样访问国内网站跟没开vpn的感觉一模一样。
2、 进入内核的流量，只做一件事：判断“应用类型”
（AI / 流媒体 / 社交 / 交易所 / 开发平台）
进入 OpenClash 内核的流量，不再做“能不能翻墙”的判断，而是只判断它属于哪一类业务应用。所有规则全部围绕「应用属性」设计，而不是围绕域名、IP 或端口堆砌，即使前端、API、CDN 频繁变动，分流逻辑依然稳定。
3、规则永远不直接绑定节点
应用 → 国家测速组 → 节点
中间永远有一层缓冲，避免规则与节点强耦合。应用规则只决定“该走哪一类出口”，节点选择全部下沉到国家测速组与冷门节点层，节点增减、机场更换、订阅更新，都不会破坏分流结构。
4、多次规则兜底
分流不依赖某一条规则或某一个规则集，而是通过
应用规则 → GFW 兜底 → 非标端口 → 漏网之鱼，多层判断逐级兜底。
5、DNS 只允许 OpenClash 决策，彻底杜绝分裂解析
所有 DNS 请求统一由 OpenClash 内核接管，结合 Fake-IP、GEOSITE、GEOIP 与策略判断，确保解析路径与实际出流路径完全一致，从根源杜绝 DNS 泄漏与解析分裂问题。
我的硬件软件配置
主机名
Openwrt
型号
QEMU Standard PC (Q35 + ICH9, 2009)
架构
12th Gen Intel(R) Core(TM) i5-12400 x 4C 4T (2496.000MHz)
目标平台
x86/64
固件版本
Kwrt 24.10.4 10.30.2025 by Kiddin’ / LuCI openwrt-24.10 branch 26.292.66247~75e41cb
内核版本
6.6.110
我使用的KWRT固件下载：
kwrt-10.30.2025-x86-64-generic-squashfs-combined.img.gz
我配置使用的机场：
bluetile专线机场>>          nicecloud专线机场>>
星云机场>>                          山水云机场>>       
xxai机场>>
🛑精细分流配置好的自定义模版链接
方案A：https://raw.githubusercontent.com/usbog232/clashmetadingyue/main/metafenliu.ini 
方案B:(应用层增加单节点手动和自动选择)
https://raw.githubusercontent.com/usbog232/clashmetadingyue/main/new-meta.ini
如果不添加或删减分流应用直接复制上方链接，修改点开上方链接复制下来去编辑。
上方使用的是本地geo规则集，需开启geo定时更新，若部分应用无法匹配，请在代码内添加url 规则集。
yaml规则集地址1：https://github.com/MetaCubeX/meta-rules-dat/tree/meta/geo-lite/geosite
yaml规则集地址2：https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Clash/OKX
视频18:13处演示中url替换操作失误 ，链接替换是下方截图部分，要保留clash- Domain：
￼
我的组网方式
￼
我的软路由其他项设置截图（大部分基本保持默认状态）
￼
￼
￼
￼
￼
￼
￼
￼
dns泄漏测试网站：ipleak.net>>
什么情况表示dns泄漏？
因为软路由有分流，出现非中国 其他地区的dns情况，并不是dns泄漏。
❌ 泄漏情况 1：出现中国运营商
哪怕只有一个：
• ￼
 China Telecom
• ￼
 China Unicom
• ￼
 China Mobile
• ￼
 Alibaba DNS / 腾讯 DNS
• 114.114.114.114
• 223.5.5.5
❌ 泄漏情况 2：IP / DNS 国家不合理
比如：
你设定的策略
实际结果
全部走代理
DNS 却是中国
国外域名
直连 DNS
￼
❌ 泄漏情况 3：出现内网 IP
例如：
• 192.168.x.1
• 10.x.x.x
说明：
• DNS 没被劫持
• 客户端绕过了软路由
只要没出现真实 ISP / 中国 DNS → 就不是泄漏
国家不同 ≠ 泄漏
节点不同 ≠ 泄漏
DNS 多个 ≠ 泄漏
出现本地 ISP 才是
关于adobe 屏蔽 
因为adobe国内也能访问，现在我们是设置了绕过大陆，相当于国内adobe得域名或ip被直接放行走的直连了，现在配置中屏蔽的只是dboe国外的域名，没经过内核的域名和ip屏不不到，如果想要全屏蔽，还需要在openclash中的 【流量控制】菜单下的【绕过指定区域 IPv4 黑名单】旁边窗口中加上 adobe的域名，添加这几个：
adobe.com
adobelogin.com
adobe.io
behance.net
（让adobe国内的ip域名都经过内核，这样就可以去面板上控制他了）
￼
￼
自己配置可搭配的个性图标
🚀 代理 / 节点 / 出口
🧭 手动选择
♻️ 自动选择
🧪 测试 / 实验
🛫 出口流量
🛬 回国 / 直连
🧯 兜底 / 应急
🐟 漏网之鱼
🔀 特殊流量 / 非标端口
⛔ 拦截 / REJECT
🌍 冷门地区 / 其他国家
🧊 冷备 / 低频使用
🛰 远端 / 海外
🪐 特殊线路 / 不稳定
🏝 小众地区
🌍 冷门节点
🪐 冷门节点
🍎 Apple
🪟 Microsoft
🔍 Google
🎬 YouTube
🎥 Netflix
🎮 Steam
📺 Disney+
🎵 Spotify
📸 Instagram
🐦 X / Twitter
🎶 TikTok
🤖 ChatGPT
🧠 AI
🧩 OpenAI
📦 GitHub
🧑‍💻 开发者
🛠 工具
📡 API
📨 Telegram
💬 即时通讯
🌐 社交媒体
📞 通话 / VoIP
☁️ 云服务
🧱 防火墙
🛡 安全
🔐 加密
📡 网络
🧬 DNS
🚫 禁止 / 拦截
