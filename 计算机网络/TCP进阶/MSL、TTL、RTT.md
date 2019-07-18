MSL：TCP报文的最大存活时间，RFC 793规定为2分钟，但是实际linux默认为30s；
TTL：IP数据包的剩余存活时间；
RTT：TCP报文的往返时间；
TIME_WAIT：2MSL，可靠的关闭全双工的TCP连接，使旧的数据包点消逝；不至于影
响下次通信；