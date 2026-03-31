mixed-port: 7890
allow-lan: false
bind-address: 127.0.0.1
mode: rule
log-level: info
ipv6: true
unified-delay: true
tcp-concurrent: true

external-controller: 127.0.0.1:9090
secret: ""

profile:
  store-selected: true
  store-fake-ip: true

dns:
  enable: true
  listen: 0.0.0.0:1053
  ipv6: true
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  use-hosts: true
  respect-rules: true
  default-nameserver:
    - 119.29.29.29
    - 223.5.5.5
  nameserver:
    - 119.29.29.29
    - 223.5.5.5
  fallback:
    - https://1.1.1.1/dns-query
    - https://dns.google/dns-query
  fallback-filter:
    geoip: true
    geoip-code: CN

proxy-providers:
  WestData:
    type: http
    url: "https://wd-red.com/subscribe/yvymjb-2lqx83a0-MbvEuTK2uNww"
    interval: 3600
    path: ./providers/westdata.yaml
    health-check:
      enable: true
      url: http://www.gstatic.com/generate_204
      interval: 300

  OuONetwork:
    type: http
    url: "https://oss5-china-south-bucket.selectgroup.click/api/graphql?token=9b1fda247a0b9432d836fe7ce8849f5a"
    interval: 3600
    path: ./providers/ouonetwork.yaml
    health-check:
      enable: true
      url: http://www.gstatic.com/generate_204
      interval: 300

  XFLTD:
    type: http
    url: "https://get.cctvclient.cn/cctv/user/client/get?token=NTczMjcxOjYwZTg4Yjc0NzljYTI0MTIyMTVmZGE0YWE1NjZiODg4MWY0ZDYyYzQ"
    interval: 0
    path: ./providers/xfltd.yaml
    health-check:
      enable: true
      url: http://www.gstatic.com/generate_204
      interval: 300

proxy-groups:
  - name: 选择节点
    type: select
    proxies:
      - 香港
      - 台湾
      - 日本
      - 新加坡
      - 美国

  - name: 香港
    type: url-test
    use:
      - WestData
      - OuONetwork
    filter: "(?i)(港|HK|Hong)"
    url: http://www.gstatic.com/generate_204
    interval: 300
    tolerance: 50

  - name: 台湾
    type: url-test
    use:
      - WestData
      - OuONetwork
      - XFLTD
    filter: "(?i)(台|TW|Tai)"
    url: http://www.gstatic.com/generate_204
    interval: 300
    tolerance: 50

  - name: 日本
    type: url-test
    use:
      - WestData
      - OuONetwork
    filter: "(?i)(日本|川日|东京|大阪|泉日|埼玉|沪日|深日|JP|Japan)"
    url: http://www.gstatic.com/generate_204
    interval: 300
    tolerance: 50

  - name: 新加坡
    type: url-test
    use:
      - WestData
      - OuONetwork
    filter: "(?i)(新加坡|坡|狮城|SG|Singapore)"
    url: http://www.gstatic.com/generate_204
    interval: 300
    tolerance: 50

  - name: 美国
    type: url-test
    use:
      - WestData
      - OuONetwork
    filter: "(?i)(美|波特兰|达拉斯|俄勒冈|凤凰城|费利蒙|硅谷|拉斯维加斯|洛杉矶|圣何塞|圣克拉拉|西雅图|芝加哥|US|United States)"
    url: http://www.gstatic.com/generate_204
    interval: 300
    tolerance: 50

rule-providers:
  AdGuardSDNSFilter:
    type: http
    behavior: domain
    url: "https://rule.kelee.one/Clash/AdGuardSDNSFilter.yaml"
    path: ./ruleset/adguard.yaml
    interval: 86400

  Advertising:
    type: http
    behavior: domain
    url: "https://rule.kelee.one/Clash/Advertising.yaml"
    path: ./ruleset/advertising.yaml
    interval: 86400

  BlockHttpDNS:
    type: http
    behavior: domain
    url: "https://rule.kelee.one/Clash/BlockHttpDNS.yaml"
    path: ./ruleset/blockhttpdns.yaml
    interval: 86400

  Privacy:
    type: http
    behavior: domain
    url: "https://rule.kelee.one/Clash/Privacy.yaml"
    path: ./ruleset/privacy.yaml
    interval: 86400

  Hijacking:
    type: http
    behavior: domain
    url: "https://rule.kelee.one/Clash/Hijacking.yaml"
    path: ./ruleset/hijacking.yaml
    interval: 86400

  ChinaMaxNoIP:
    type: http
    behavior: domain
    url: "https://rule.kelee.one/Clash/ChinaMaxNoIP.yaml"
    path: ./ruleset/chinamax.yaml
    interval: 86400

  ChinaIPs:
    type: http
    behavior: ipcidr
    url: "https://rule.kelee.one/Clash/ChinaIPs.yaml"
    path: ./ruleset/chinaip.yaml
    interval: 86400

  ChinaDNS:
    type: http
    behavior: domain
    url: "https://rule.kelee.one/Clash/ChinaDNS.yaml"
    path: ./ruleset/chinadns.yaml
    interval: 86400

rules:
  - RULE-SET,AdGuardSDNSFilter,REJECT
  - RULE-SET,Advertising,REJECT
  - RULE-SET,BlockHttpDNS,REJECT
  - RULE-SET,Privacy,REJECT
  - RULE-SET,Hijacking,REJECT

  - IP-ASN,4134,DIRECT
  - IP-ASN,4809,DIRECT
  - IP-ASN,4812,DIRECT
  - IP-ASN,4811,DIRECT

  - IP-ASN,4837,DIRECT
  - IP-ASN,9929,DIRECT
  - IP-ASN,17622,DIRECT

  - IP-ASN,9808,DIRECT
  - IP-ASN,56040,DIRECT
  - IP-ASN,56041,DIRECT
  - IP-ASN,24400,DIRECT

  - IP-ASN,137699,DIRECT

  - GEOIP,CN,DIRECT
  - RULE-SET,ChinaMaxNoIP,DIRECT
  - RULE-SET,ChinaDNS,DIRECT
  - RULE-SET,ChinaIPs,DIRECT

  - MATCH,选择节点
