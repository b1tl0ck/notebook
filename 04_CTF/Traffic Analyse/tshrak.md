---
tags:
  - wireshark
create_time: 2024-10-18 10:11
modified_time: 2024-10-18 10:11
status: complete
---
当在Linux中运行tshark命令时，可以使用不同的选项和参数来控制其行为。以下是一些常见的tshark命令选项的解释：

Tshark -r misc1. Pcap -T fields -e data -Y "icmp. Type == 0" > 1. Txt

```markdown
-i <interface>：指定要捕获数据包的网络接口。例如，-i eth0表示使用eth0接口进行捕获，-i any表示捕获所有可用接口上的数据包。

-r <filename>：从指定的数据包文件中读取数据包进行分析。可以使用该选项来分析之前保存的数据包文件。

-Y <filter>：使用过滤器来限制显示满足条件的数据包。过滤器可以基于各种协议、源/目的IP地址、端口号、数据包大小等进行设置。例如，tshark -Y "ip.src == 192.168.0.1：仅显示源IP地址为192.168.0.1的数据包;tshark -Y "tcp.port == 80：仅显示目的或源端口为80的TCP数据包;tshark -Y "http.request.method == GET：仅显示HTTP请求方法为GET的数据包。

-T <output_format>：指定输出格式，将分析结果以不同的格式输出。可以选择的输出格式包括pdml、json、ps、psml等。例如，-T pdml将数据包以PDML格式输出到控制台。

-z <statistics>：执行统计和分析操作，并输出相关的统计信息。可以进行各种类型的统计，如I/O图、协议分层统计、TCP流会话等。例如，-z io,phs将显示I/O图和协议分层统计。

-c <count>：指定捕获数据包的数量。例如，-c 100表示仅捕获前100个数据包。

-w <filename>：将捕获到的数据包写入指定的文件中。可以使用该选项将捕获的数据包保存到文件中以供后续分析。

-f <capture_filter>：设置捕获过滤器，只捕获满足特定条件的数据包。捕获过滤器可以根据协议、端口、源/目的IP地址等进行设置。例如，tshark -i eth0 -f ‘tcp dst port 80’表示抓取http包

-V：显示详细的数据包信息，包括各个协议的字段和值。该选项会输出较为详细的数据包解析结果。

-e <field>：指定要显示的特定字段。可以使用该选项来仅显示感兴趣的字段信息。例如，-e http.request.method将仅显示HTTP请求方法字段的值。

-q：以静默模式运行，减少输出的冗余信息。在进行自动化脚本或批处理时，可以使用该选项来减少不必要的输
```



