# my-blindsql-script
# My Blind SQLi Script (时间盲注自动化脚本)

这是一个用 Python 写的时间盲注自动化爆破脚本。主要针对 PortSwigger 靶场里的时间盲注关卡开发，用来自动猜解管理员密码。

## 为什么写这个脚本？

一开始我是手工在 Burp Suite 里发请求，但是时间盲注每次都要等好几秒，手工测试效率太低，所以写了这个脚本让电脑自己去跑。

## 核心特点

- **二分查找优化**：没有用传统的从 a 到 z 挨个遍历，而是使用了二分查找算法（基于 ASCII 码判断大小），猜解速度大幅提升。
- **防封禁处理**：加入了随机延迟（time.sleep），防止发包太快被靶场的 WAF 封禁 IP。
- **绕过 HTTP 编码坑**：使用自定义 Headers 强行发送 Cookie，避免了 Python requests 库自动进行 URL 编码导致 SQL 注入失效的问题。

## 怎么用？

1. 确保你安装了 Python 3 以及 requests 库（`pip install requests`）。
2. 把脚本里的靶场 URL 和 Cookie 换成你自己的。
3. 运行：`python blind_sqli.py`

## 踩坑记录（开发心得）

- 靶场的 Session 可能会过期，写脚本的时候要注意获取最新状态。
- 用 Python 的 requests 库直接传 cookies 参数会被自动编码，导致注入语句失效，必须用 headers 字典手动拼接。
- 判断响应时间的时候，不能卡得太死。比如 pg_sleep(3)，阈值设成大于 2.5 秒比较稳，能有效防止网络波动造成的误判。
