# wireguard-mix

基于 wireguard-go,做了一些小修改，规避防火墙的检测。

暂时只做了一点点改动，但是满足需求了

- 修改 Type 为随机 uint32 (暂时是静态的)

## 构建

需要 golang 1.20+

```bash
go build -o wireguard-mix
```

### 跨平台编译

Go 原生支持跨平台编译，通过设置 `GOOS` 和 `GOARCH` 环境变量即可。

**支持的操作系统：** Windows, macOS (darwin), Linux, FreeBSD

**支持的架构：** amd64, arm64, arm, 386, mips, mipsel 等

**示例命令：**

```bash
# Linux amd64
GOOS=linux GOARCH=amd64 go build -o wireguard-mix-linux-amd64

# macOS arm64 (Apple Silicon)
GOOS=darwin GOARCH=arm64 go build -o wireguard-mix-macos-arm64

# FreeBSD amd64
GOOS=freebsd GOARCH=amd64 go build -o wireguard-mix-freebsd-amd64

# Linux mipsel (软路由常用)
GOOS=linux GOARCH=mipsle GOMIPS=softfloat go build -o wireguard-mix-linux-mipsel
```

## 用法

这样子开接口：

```bash
wireguard-mix wg0
```

然后就可以用 [`wg(8)`](https://git.zx2c4.com/wireguard-tools/about/src/man/wg.8)，`ip(8)` 和`ifconfig(8)` 配置了

## wg-quick-mix

为了方便使用, wg-quick 也给出了魔改, 位于项目根目录下

但是对 2020 及以前的 wg 会有一些适配问题，如果使用出现问题，可以尝试 `apt upgrade`

```bash
sudo apt update
sudo apt install -y wireguard-tools
sudo mv /usr/bin/wg-quick /usr/bin/wg-quick.bak
sudo cp wireguard-mix/wg-quick /usr/bin/wg-quick
sudo chmod +x /usr/bin/wg-quick
```

魔改版的 wg-quick 可以使用 WgBin 来选择 wireguard 实例，留空则与原版行为一致

```conf
[Interface]
ListenPort = port
PrivateKey = PrivateKey 
WgBin = wireguard-mix
```

WgBin 可以直接填写路径，也这样写先丢到 path 里去再玩

```bash
sudo cp wireguard-mix /usr/local/bin/
```

## wireguard-go 的 License

    Copyright (C) 2017-2025 WireGuard LLC. All Rights Reserved.
    
    Permission is hereby granted, free of charge, to any person obtaining a copy of
    this software and associated documentation files (the "Software"), to deal in
    the Software without restriction, including without limitation the rights to
    use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies
    of the Software, and to permit persons to whom the Software is furnished to do
    so, subject to the following conditions:
    
    The above copyright notice and this permission notice shall be included in all
    copies or substantial portions of the Software.
    
    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
    IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
    FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
    AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
    LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
    OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
    SOFTWARE.
