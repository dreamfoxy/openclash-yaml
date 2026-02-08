以下是将内容转换为美观的 Markdown 格式的结果：

```markdown
OpenClash 精细分流配置模板  
作者：xiaoji| 插件分享/魔珐翻墙  
2026年02月08日更新  
核心优势  
四层流量精准分流  
1. 系统层预过滤
- 绕过 80% 国内流量（私网/IPv4 中国段/本地服务）  
- 剩余流量通过「中国域名+IP 补丁」二次兜底直连  
- 国内网站访问体验≈未开启 VPN  

2. 应用属性分流
```ini
规则集逻辑示例
- 规则类型: 应用属性（非域名/IP堆砌）
- 覆盖场景: 
• 流媒体/社交/AI/交易所  
• 开发平台/云服务/非标端口
```  
- 动态适应 API/CDN 波动，稳定性提升 300%  

3. 节点解耦架构
```mermaid
graph LR
A[应用规则] --> B[国家测速组]
B --> C[冷门节点层]
C --> D[动态节点池]
```  
- 机场订阅更新/节点增减 零配置扰动
️ 配置方案  
分流模板（二选一）  
| 方案 | 链接 | 特点 |  
|------|------|------|  
| 基础版| meta分流.ini(https://raw.githubusercontent.com/usbog232/clashmetadingyue/main/metafenliu.ini) | 本地 GEO 规则 + 自动更新 |  
| 增强版| new-meta.ini(https://raw.githubusercontent.com/usbog232/clashmetadingyue/main/new-meta.ini) | 支持单节点手动选择 |  

️ 必做设置  
```yaml
DNS 防泄漏配置
dns:
enable: true
enhanced-mode: fake-ip
nameserver:
- 'https://1.1.1.1/dns-query'
fallback-filter: { geoip: true }
```

特殊场景优化  
Adobe 服务全局代理
```bash
在「绕过指定区域IPv4黑名单」添加：
adobe.com
adobelogin.com 
adobe.io
```  
原理：强制国内 Adobe 流量经内核分流  
防 DNS 泄漏验证  
合格场景（ipleak.net 测试）  
- 仅出现 中国运营商 DNS：  
```plaintext
114.114.114.114 | 223.5.5.5 | 中国电信/联通/移动
```  
- 无境外 DNS / 内网 IP（如 `192.168.x.x`）  

泄漏特征  
| 类型 | 表现 | 风险 |  
|------|------|------|  
| 国家错位 | 直连策略但出现境外 DNS | 隐私暴露 |  
| 内网 IP | 出现 `10.x.x.x` 等地址 | 路由劫持 |  
️ 环境信息  
硬件配置  
```properties
主机名: Openwrt  
架构: x86/64 (QEMU 标准PC)  
CPU: 12代 i5-12400 @ 2.5GHz  
固件: Kwrt 24.10.4 (内核 6.6.110)  
```
️ KWRT固件下载(kwrt-10.30.2025-x86-64-generic-squashfs-combined.img.gz)  

推荐机场  
```diff
- bluetile专线 | Nicecloud专线 | 星云机场 
- 山水云 | xxai机场
```
规则集资源  
官方仓库  
```plaintext
1. Meta 核心规则集  
https://github.com/MetaCubeX/meta-rules-dat/tree/meta/geo-lite/geosite  
2. 第三方增强规则  
https://github.com/blackmatrix7/iosrulescript/tree/master/rule/Clash/OKX  
```

策略组图标库  
| 场景 | 图标 | 场景 | 图标 |  
|------|------|------|------|  
| 代理出口 |  | 直连回国 |  |  
| AI 服务 |  | 流媒体 |  |  
| 加密流量 |  | 拦截 |  |  

>  提示：图标需在 OpenClash 面板中手动绑定策略组  
>  配置精髓：DNS 决策权 100% 交予 OpenClash，结合 Fake-IP + GEOSITE 彻底杜绝分割解析！
```
