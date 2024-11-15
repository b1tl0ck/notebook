---
tags:
  - 代理
create_time: 2023-12-10 12:25
modified_time: 2024-05-30 15:17
status: complete
---
### frps

```ini
[common]
bind_addr = 0.0.0.0
bind_port = 8080

dashboard_addr = 0.0.0.0
dashboard_port = 7882
dashboard_user = jdih
dashboard_pwd = jdsiojhilsld

log_file = ./frps.log
log_level = info
log_max_days = 30
disable_log_color = false

authentication_method = token
authenticate_heartbeats = true
authenticate_new_work_conns = true
token = 829hiuKDJ9kjhdyg
```

### frpc

```ini
[common]
server_addr = xxx.xxx.xxx.xxx
server_port = 8080

authenticate_heartbeats = true
authenticate_new_work_conns = true
token = 829hiuKDJ9kjhdyg

protocol = tcp

tls_enable = true
disable_custom_tls_first_byte = true

[test]
type = tcp
remote_port = 18081
plugin = socks5
plugin_user = dsjfkljlkjl
plugin_passwd = dskijoiji929n
use_encryption = true
use_compression = true
```

