# DNS 服务器

[![English][1]][2] [![German][3]][4] [![Russian][5]][6]

[1]: ../resources/english.svg
[2]: https://www.v2ray.com/en/configuration/dns.html
[3]: ../resources/german.svg
[4]: https://www.v2ray.com/de/configuration/dns.html
[5]: ../resources/russian.svg
[6]: https://www.v2ray.com/ru/configuration/dns.html

V2Ray 内置了一个 DNS 服务器，可以将 DNS 查询根据路由设置转发到不同的远程服务器中。

示例配置：

```javascript
{
  "hosts": {
    "baidu.com": "127.0.0.1"
  },
  "servers": [
      "8.8.8.8",
      "8.8.4.4",
      "localhost"
  ],
  "clientIp": "1.2.3.4",
}
```

其中：

* `hosts`: 静态 IP 列表，其值为一系列的"域名":"IP"，IP 可以是 IPv4 或者 IPv6。在解析域名时，如果域名匹配这个列表中的某一项，则解析结果为该项的 IP，而不会使用下述的 servers 进行解析。域名的格式如下：
  * 形如 `"v2ray.com"`: 被解析的域名需要和此域名完全一致；
  * 形如 `"domain:v2ray.com"`: 被解析的域名可以是此域名或是其子域名。
* `servers`: 指定一个 DNS 服务器列表，可选的值如下。如果配置了多个 DNS 服务器，则会按从上到下的优先级顺序进行 DNS 查询。
  * `"IP"`: 目前只支持传统的端口为 53 的 DNS 服务器；
  * `"localhost"`: 这是一个特殊值，表示使用本机预设的 DNS 配置。当使用 localhost 时，本机的 DNS 请求不受 V2Ray 控制，需要额外的配置才可以使 DNS 请求由 V2Ray 转发。
* `clientIp`: 当前系统的 IP 地址，用于 DNS 查询时，通知服务器客户端的所在位置。不能是私有地址。

若要使 DNS 服务生效，需要配置路由功能中的 `domainStrategy`。

由此 DNS 服务器所发出的 DNS 查询请求，会自动根据路由配置进行转发，无需额外配置。

## 查询策略 {#strategy}

V2Ray 内置的 DNS 会在单个 DNS 请求中同时查询 A 和 AAAA 记录。由于这种方式并不是所有 DNS 服务器都支持，V2Ray 仅对以下 DNS 服务器同时查询 A 和 AAAA 记录，对其它所有的服务器只查询 A 记录。

```text
8.8.8.8
8.8.4.4
9.9.9.9
```

## 小贴士 {#tips}

* 推荐使用本机 DNS 配合一个额外的 DNS 服务器来做 DNS 查询，如 [CoreDNS](https://coredns.io/)。
