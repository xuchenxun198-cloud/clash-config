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
    url: "https://sub.xfltd.me/cctv/user/client/get?token=fac5c3c4a7f8977c5d35b3e80236bb11"
    interval: 0
    path: ./providers/xfltd.yaml
    health-check:
      enable: true
      url: http://www.gstatic.com/generate_204
      interval: 300

  Mitce:
    type: http
    url: "https://app.mitce.net/?sid=570892&token=srvzbyfc"
    interval: 3600
    path: ./providers/mitce.yaml
    health-check:
      enable: true
      url: http://www.gstatic.com/generate_204
      interval: 300

  Valtrogen:
    type: http
    url: "https://api.valconfig.com/sub/?access_key=738486c63b18abc7d693296b1db87104"
    interval: 3600
    path: ./providers/valtrogen.yaml
    health-check:
      enable: true
      url: http://www.gstatic.com/generate_204
      interval: 300

  良心云:
    type: http
    url: "https://liangxin.xyz/api/v1/liangxin?OwO=7b48b9fb2b42d566d03346c52258344b"
    interval: 3600
    path: ./providers/liangxin.yaml
    health-check:
      enable: true
      url: http://www.gstatic.com/generate_204
      interval: 300

  BestTelegram:
    type: http
    url: "https://suuuuuuub.besttelecom.cc/api/v1/client/subscribe?token=3c38f1f6ee4915cadb9862fd37e6d95e"
    interval: 3600
    path: ./providers/besttelegram.yaml
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
      - Mitce
      - Valtrogen
      - 良心云
      - BestTelegram
      - XFLTD
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
      - Mitce
      - Valtrogen
      - 良心云
      - BestTelegram
    filter: "(?i)(台|TW|Tai)"
    url: http://www.gstatic.com/generate_204
    interval: 300
    tolerance: 50

  - name: 日本
    type: url-test
    use:
      - WestData
      - OuONetwork
      - Mitce
      - Valtrogen
      - 良心云
      - BestTelegram
      - XFLTD
    filter: "(?i)(日本|川日|东京|大阪|泉日|埼玉|沪日|深日|JP|Japan)"
    url: http://www.gstatic.com/generate_204
    interval: 300
    tolerance: 50

  - name: 新加坡
    type: url-test
    use:
      - WestData
      - OuONetwork
      - Mitce
      - Valtrogen
      - 良心云
      - BestTelegram
      - XFLTD
    filter: "(?i)(新加坡|坡|狮城|SG|Singapore)"
    url: http://www.gstatic.com/generate_204
    interval: 300
    tolerance: 50

  - name: 美国
    type: url-test
    use:
      - WestData
      - OuONetwork
      - Mitce
      - Valtrogen
      - 良心云
      - BestTelegram
      - XFLTD
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

  ChinaASN:
    type: http
    behavior: classical
    url: "https://rule.kelee.one/Clash/ChinaASN.yaml"
    path: ./ruleset/chinaasn.yaml
    interval: 86400

rules:
  - RULE-SET,AdGuardSDNSFilter,REJECT
  - RULE-SET,Advertising,REJECT
  - RULE-SET,ChinaASN,DIRECT
  - RULE-SET,ChinaMaxNoIP,DIRECT
  - RULE-SET,ChinaDNS,DIRECT
  - RULE-SET,ChinaIPs,DIRECT
  - GEOIP,CN,DIRECT
  - MATCH,选择节点
