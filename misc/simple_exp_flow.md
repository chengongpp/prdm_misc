# 一个简单的批量扫描和利用流程

随手抽一个国产框架，这个框架的管理端鉴权非常辣鸡，甚至几乎不鉴权：

> 此处略去。

随手抽几个国内IP段：

```bash
(<china_ip.txt) | grep 24$ | shuf -n 5 > ranges.txt
```

把IP段扩充完整，筛去组播地址：

```python
#!/usr/bin/env python3
import ipaddress
import sys
if __name__ == "__main__":
    if len(sys.argv) < 2:
        print("Usage: ipxpnd.py")
        sys.exit(1)
    ipranges = sys.stdin.read().splitlines(keepends=False)
    ipranges = [ipaddress.ip_network(iprange) for iprange in ipranges]
    for iprange in ipranges:
        for ip in iprange.hosts():
            sys.stdout.write(str(ip))
            sys.stdout.write("\n")
    sys.exit(0)
```

```bash
(<ranges.txt) | ./ipxpnd > ips.txt
```

乱序IP，避免被waf认为我们在连续扫描，虽然感觉还是会被认出来

```bash
(<ips.txt) | shuf > ips_shuf.txt
```

扫描其中8080端口开放的IP

直接对着框架常用的管理端口开扫。

```bash
for ip in (<ips_shuf.txt); do ; done
```