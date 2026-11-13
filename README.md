# AdGuard Home 国内外 DNS 分流

域名来自 [dnsmasq-china-list](https://github.com/felixonmars/dnsmasq-china-list) 的 `accelerated-domains.china.conf`。命中清单的查询走阿里 DoT（`tls://dns.alidns.com`），其余走 Cloudflare DoH（`https://cloudflare-dns.com/dns-query`）。

GitHub Actions 每天北京时间 06:23 检查上游。也可以在 [Actions](https://github.com/isyekong/adguardhome-china-dns-rules/actions/workflows/publish-rules.yml) 手动运行。规则变化时发布新版本。[下载 upstreams.txt](https://github.com/isyekong/adguardhome-china-dns-rules/releases/latest/download/upstreams.txt)

## OPNsense 用法

将 `upstreams.txt` 下载到 `/usr/local/AdGuardHome/`。如果原来有内网转发规则，把它们一并写入本地文件。首次配置时停止 AdGuard Home 并备份 `AdGuardHome.yaml`，在已有的 `dns:` 段设置以下字段，再启动服务。

```yaml
  upstream_dns_file: /usr/local/AdGuardHome/upstreams.txt
```

AdGuard Home 会从文件读取默认上游和域名规则。更新本地文件后执行 `service adguardhome restart` 重新加载。用查询日志检查 `baidu.com` 和 `example.org` 的上游是否分别为阿里 DoT 和 Cloudflare DoH。两组上游均需要能从 OPNsense 访问。

[AdGuard Home 上游文件说明](https://github.com/AdguardTeam/AdGuardHome/wiki/Configuration#loading-upstreams-from-file)
