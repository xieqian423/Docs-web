# TCP的连接和释放

## TCP三次握手
![TCP三次握手](/images/TCP三次握手.png)

在第一次消息发送中，A随机选取一个序列号作为自己的初始序号发送给B；
第二次消息B使用ack对A的数据包进行确认，因为已经收到了序列号为x的数据包，准备接收序列号为x+1的包，所以ack=x+1，同时B告诉A自己的初始序列号，就是seq=y；
第三条消息A告诉B收到了B的确认消息并准备建立连接，A自己此条消息的序列号是x+1，所以seq=x+1，而ack=y+1是表示A正准备接收B序列号为y+1的数据包。

seq是数据包本身的序列号；ack是期望对方继续发送的那个数据包的序列号。
ACK=1表示确认号有效，为0表示报文中不包含确认信息，忽略确认号字段。
SYN=1,表示这是一个连接请求或连接接受报文，TCP规定SYN=1的报文段，不能携带数据。

### 三次握手的原因
A发出连接请求，但是未收到确认，A再次重传请求，收到了确认建立了连接，数据传输完成后释放，A发出了两次请求。

TCP的三次握手最主要是防止已过期的连接再次传到被连接的主机。
A发送的请求报文连接段并没有丢失，而是在某个网络节点滞留较长时间，以致延误到请求释放后的某个时间到达B，
本来是一个早已失效的报文段，但是B收到了此失效连接请求报文段后，就误以为A又重新发送的连接请求报文段，
并发送确认报文段给A，同意建立连接，如果没有三次握手，那么B发送确认后，连接就建立了，而此时A没有发送建立连接的请求报文段，
于是不理会B的确认，也不会给B发送数据，而B却一直等待A发送数据，因此B的许多资源就浪费了，
采用三次握手的方式就可以防止这种事情发生，
例如刚刚，A不理会B，就不会给B发送确认，B收不到A的确认，就知道A不要求建立连接，就不会白白浪费资源，

## TCP四次挥手
![TCP四次挥手](/images/TCP四次挥手.png)

1. 当客户端把所有数据传送完毕的时候，给服务端口送一个FIN报文，请求关闭客户端到服务端的连接，ESTABLISHED 变为FIN_WAIT_1,；
2. 服务端收到客户的FIN报文后，送一个ACK确认报文给客户端，状态变为CLOSE_WAIT；
3. 客户端端收到服务的ACK报文，将FIN_WAIT_1置FIN_WAIT_2,同时，继续等待服务端发送FIN报文。
4. 当服务端数据传送也完毕的后，开始给客户端发送FIN包，发送FIN包后，其状态CLOSE_WAIT置为LAST_ACK
5. 客户端收到FIN包后，又给服务端发送ACK。发送ACK后，状态由原来的FIN_WAIT_2置为TIME_WAIT，客户在经过2MSL 时间进入CLOSE状态
6. 服务端口收到客户端发送的ACK后，由LAST_ACK也进入CLOSE

### 四次挥手的原因
由于TCP连接是全双工的，因此每个方向都必须单独进行关闭。当一方完成它的数据发送任务后就能发送一个FIN来终止这个方向的连接

### TIME_WAIT等待2MSL的原因
1. 如果A发送完ACK报文段就释放连接，无法收到B的重传FIN+ACK报文段，也无法再次发送确认，B无法正常进入CLOSED状态。
2. 以确保网络上当前连接两个方向上尚未接收的TCP报文已经全部消失。
在TIME_WAIT状态下，不允许应用程序在当前ip和端口上和之前通信的client(这个client的ip和端口号不变)建立一个新的连接。这样就能避免新的连接收到之前的ip和端口一致的连接残存在网络中的数据包。

MSL（最长报文段寿命）建议是2分钟：IP数据包能在互联网上生存的最长时间，超过这个时间IP数据包将在网络中消失
