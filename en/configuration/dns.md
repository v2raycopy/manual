# DNS

[![English][1]][2] [![Chinese][3]][4] [![German][5]][6] [![Russian][7]][8]

[1]: ../resources/english.svg
[2]: https://www.v2ray.com/en/configuration/dns.html
[3]: ../resources/chinese.svg
[4]: https://www.v2ray.com/chapter_02/04_dns.html
[5]: ../resources/german.svg
[6]: https://www.v2ray.com/de/configuration/dns.html
[7]: ../resources/russian.svg
[8]: https://www.v2ray.com/ru/configuration/dns.html

V2Ray has an internal DNS server which provides DNS relay for other components.

Configuration:

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

Where:

* `hosts`: A list of static IP addresses. Each entry has a domain name as key and IP address as value. If a DNS query targets one of the domains in this list, the corresponding IP will be returned immediately and DNS query will not be relayed. The format of the domain is:
  * Such as `"v2ray.com"`: The domain to be resolved has to equal to this domain.
  * Such as `"domain:v2ray.com"`: The domain to be resolved can be this domain or any of its sub-domains.
* `servers`: A list of DNS server addresses. If there are more than one servers, they will be queried from top down. Options for DNS address:
  * `"IP"`: An IP address whose port 53 is open for DNS query.
  * `"localhost"`: A special value that V2Ray will use DNS query from local machine.
* `clientIp`: IPv4 address of current system. This is used to notify DNS server for better IP resolution. The value can't be a private address.

To use the internal DNS service, you need to configure `domainStrategy` in [routing](routing.md).

The DNS queries relayed by this DNS service will also be dispatched based on routing settings. No extra configuration is required.

## Query strategy {#strategy}

DNS service will try to query both A and AAAA record in the same DNS message. As not all DNS servers support such query, V2Ray only sends A and AAAA query to the following DNS servers, and only send A queries to all other servers.

```text
8.8.8.8
8.8.4.4
9.9.9.9
```

## Tips {#tips}

* You are recommended to use DNS from your localhost, with a thirdparty DNS relay server, such as [CoreDNS](https://coredns.io/).
