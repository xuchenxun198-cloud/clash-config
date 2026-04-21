mixed-port: 7890
allow-lan: true
bind-address: "*"
ipv6: true
mode: rule
log-level: info
unified-delay: true
tcp-concurrent: true

profile:
  store-selected: true
  store-fake-ip: true

geodata-mode: true
geo-auto-update: true
geo-update-interval: 24

dns:
  enable: true
  ipv6: true
  respect-rules: true
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  default-nameserver:
    - 223.5.5.5
    - 119.29.29.29
  nameserver:
    - https://dns.alidns.com/dns-query
    - https://doh.pub/dns-query
  proxy-server-nameserver:
    - https://dns.google/dns-query
    - https://cloudflare-dns.com/dns-query
  nameserver-policy:
    "rule-set:ChinaDNS":
      - https://dns.alidns.com/dns-query
      - https://doh.pub/dns-query
  fake-ip-filter:
    - "*.lan"
    - "*.local"
    - "+.msftconnecttest.com"
    - "+.msftncsi.com"

proxy-groups:
  - name: 节点选择
    type: select
    proxies:
      - 自动选择
      - 香港
      - 台湾
      - 日本
      - 新加坡
      - 美国

  - name: 自动选择
    type: url-test
    url: https://www.gstatic.com/generate_204
    interval: 300
    tolerance: 50
    include-all: true

  - name: 香港
    type: url-test
    url: https://www.gstatic.com/generate_204
    interval: 300
    tolerance: 50
    include-all: true
    filter: "(?i)(港|HK|Hong)"

  - name: 台湾
    type: url-test
    url: https://www.gstatic.com/generate_204
    interval: 300
    tolerance: 50
    include-all: true
    filter: "(?i)(台|TW|Tai)"

  - name: 日本
    type: url-test
    url: https://www.gstatic.com/generate_204
    interval: 300
    tolerance: 50
    include-all: true
    filter: "(?i)(日本|川日|东京|大阪|泉日|埼玉|沪日|深日|JP|Japan)"

  - name: 新加坡
    type: url-test
    url: https://www.gstatic.com/generate_204
    interval: 300
    tolerance: 50
    include-all: true
    filter: "(?i)(新加坡|坡|狮城|SG|Singapore)"

  - name: 美国
    type: url-test
    url: https://www.gstatic.com/generate_204
    interval: 300
    tolerance: 50
    include-all: true
    filter: "(?i)(美|波特兰|达拉斯|俄勒冈|凤凰城|费利蒙|硅谷|拉斯维加斯|洛杉矶|圣何塞|圣克拉拉|西雅图|芝加哥|US|United States)"

rule-providers:
  AdGuardSDNSFilter:
    type: http
    behavior: domain
    url: "https://rule.kelee.one/Clash/AdGuardSDNSFilter.yaml"
    path: ./ruleset/AdGuardSDNSFilter.yaml
    interval: 86400

  Advertising:
    type: http
    behavior: domain
    url: "https://rule.kelee.one/Clash/Advertising.yaml"
    path: ./ruleset/Advertising.yaml
    interval: 86400

  ChinaMaxNoIP:
    type: http
    behavior: domain
    url: "https://rule.kelee.one/Clash/ChinaMaxNoIP.yaml"
    path: ./ruleset/ChinaMaxNoIP.yaml
    interval: 86400

  ChinaIPs:
    type: http
    behavior: ipcidr
    url: "https://rule.kelee.one/Clash/ChinaIPs.yaml"
    path: ./ruleset/ChinaIPs.yaml
    interval: 86400

  ChinaDNS:
    type: http
    behavior: domain
    url: "https://rule.kelee.one/Clash/ChinaDNS.yaml"
    path: ./ruleset/ChinaDNS.yaml
    interval: 86400

  ChinaASN:
    type: http
    behavior: ipcidr
    url: "https://rule.kelee.one/Clash/ChinaASN.yaml"
    path: ./ruleset/ChinaASN.yaml
    interval: 86400

rules:
  - RULE-SET,AdGuardSDNSFilter,REJECT
  - RULE-SET,Advertising,REJECT
  - RULE-SET,ChinaMaxNoIP,DIRECT
  - RULE-SET,ChinaIPs,DIRECT
  - RULE-SET,ChinaDNS,DIRECT
  - RULE-SET,ChinaASN,DIRECT
  - GEOIP,CN,DIRECT
  - MATCH,节点选择
