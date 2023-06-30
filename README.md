# Tron Profanity

![https://github.com/GG4mida/profanity-tron/actions](https://img.shields.io/github/actions/workflow/status/GG4mida/profanity-tron/release.yml)
![https://github.com/GG4mida/profanity-tron](https://img.shields.io/badge/baseon-gpu-yellowgreen.svg)
![https://github.com/GG4mida/profanity-tron](https://img.shields.io/badge/language-c,c++-orange.svg)
![https://github.com/GG4mida/profanity-tron](https://img.shields.io/badge/platform-windows,linux-yellow.svg)
![https://t.me/jackslowfak](https://img.shields.io/badge/telegram-@jackslowfak-blue.svg)

波场（TRON）靓号生成器，利用 `gpu` 进行加速。代码开源，安全可靠 🔥

<img width="100%" src="https://github.com/GG4mida/profanity-tron/blob/main/screenshot/demo.png?raw=true"/>

> Fbi Warning 1: 该程序仅用于学习交流，请勿用于非法用途。

> Fbi Warning 2: 本程序仅在本仓库发布并更新，请勿下载运行其它来路不明的版本，由此造成的一切损失，请自行承担。

## 广告

需要以太坊（ETH）靓号生成的，请移步：[profanity-ether](https://github.com/GG4mida/profanity-ether)

## 说明

- 本程序基于以太坊地址生成器：[profanity](https://github.com/johguse/profanity) 修改而来，同时修复了原程序私钥可爆破的问题。可参考下方 `安全` 章节说明。
- 即使本程序已修复原程序已公开漏洞，但仍然建议你对生成的地址进行 `多签` 后再使用。多签后的地址，可保证 `100%` 安全，关于如何多签，请自行谷歌。 

## 运行

### Windows

下载 `dist/windows.zip`，解压，然后直接运行 `start.bat` 即可。

> 请参考下文 `命令 & 参数` 章节说明，自行编辑 `start.bat` 配置运行参数。

### Mac

下载源码，然后定位到目录下执行 `make`，接着运行 `./profanity.x64 ...` 即可。

## 命令介绍

```bash
Usage: ./profanity [OPTIONS]

  Help:
    --help              Show help information

  Modes with arguments:
    --matching          Matching input, file or single address.

  Matching configuration:
    --prefix-count      Minimum number of prefix matches, default 0
    --suffix-count      Minimum number of suffix matches, default 6
    --quit-count        Exit the program when the generated number is greater than, default 0

  Device control:
    --skip              Skip device given by index

  Output control:
    --output            The file to output the results to
    --post              The url to post the results to

Examples:

  ./profanity --matching profanity.txt
  ./profanity --matching profanity.txt --skip 1
  ./profanity --matching profanity.txt --output result.txt
  ./profanity --matching profanity.txt --post http://127.0.0.1:7001/api
  ./profanity --matching profanity.txt --prefix-count 1 --suffix-count 8
  ./profanity --matching profanity.txt --prefix-count 1 --suffix-count 10 --quit-count 1
  ./profanity --matching TUqEg3dzVEJNQSVW2HY98z5X8SBdhmao8D --prefix-count 2 --suffix-count 4 --quit-count 1

About:

  Profanity is a vanity address generator for Tron: https://tron.network
  The software is modified based on ethereum profanity: https://github.com/johguse/profanity
  Please make sure the program you are running is download from: https://github.com/GG4mida/profanity-tron
  Author: telegram -> @jackslowfak

Fbi Warning:

  Before using a generated vanigity address, always verify that it matches the printed private key.
  And always multi-sign the address to ensure account security.
```

### 命令说明

|  参数  | 说明  |
|  ----  | ----  |
|--help|查看帮助说明|
|--matching|固定写法，后面跟上匹配规则文件|
|--prefix-count|最少匹配前缀位数，默认 0。比如你可以配置为 8，那就匹配 8 个 T 的地址|
|--suffix-count|最少匹配后缀位数，默认 6。比如你可以配置为 10，那就匹配 10 位的后缀（10位其实挺难的，估计要跑到天荒地老 :<）|
|--quit-count|生成的地址达到指定的数量，即退出程序。比如你就想匹配一个地址，那就配置为 1。系统默认退出数量为 120|
|--output|将生成的地址输出到文件（追加）。一行一个，格式如：privatekey,address|
|--post|将生成的地址，发送到（GET）指定的 url，每生成一条就会发送一次。数据格式为：privatekey=xx&address=yy。这个配置主要便于其它系统的集成|
|--skip|跳过指定索引的 gpu 设备，如启动软件出现异常，请使用此参数跳过设备集成显卡|

### 匹配规则

> 目前 `--matching` 参数支持指定单个地址或一个文件。

#### 单个地址

```bash
./profanity --matching TUqEg3dzVEJNQSVW2HY98z5X8SBdhmao8D --prefix-count 2 --suffix-count 4
```

#### 文件

```bash
./profanity --matching profanity.txt --prefix-count 2 --suffix-count 4 --quit-count 1
```

匹配文件里面，目前支持两种写法，可参考内置 `profanity.txt`。举个例子：

```plaintext
TTTTTTTTTTZZZZZZZZZZ
TUqEg3dzVEJNQSVW2HY98z5X8SBdhmao8D
```

上面这两条匹配规则：
- 第一条，是匹配以字母 `Z` 结尾的靓号。
- 第二条，是匹配这条地址的前后 `10` 位，实际运行的时候，会自动修正为：TUqEg3dzVE8SBdhmao8D。

有了匹配规则，再结合 `prefix-count`（最少匹配前缀数量） & `suffix-count`（最少匹配后缀数量），即可实现任意规则靓号地址生成。

## 开发

> 这里主要讲讲如何构建 `windows` 平台的 `exe 可执行程序`。`mac` 机器理论上可直接 `make`，然后执行就行。

> 本人在开发的时候，是买了一台阿里云 `v100 gpu卡` + `windows server 2022` 的抢占式实例。如果已经有对应的开发环境，可以不用花这个钱。

### 连接到服务器

> ssh，你懂的。

### 安装显卡驱动

1. 打开 `nvidia` 驱动下载网站：[https://www.nvidia.cn/Download/index.aspx?lang=cn](https://www.nvidia.cn/Download/index.aspx?lang=cn)

2. 根据服务器配置搜索驱动，然后下载：

<img width="100%" src="https://github.com/GG4mida/profanity-tron/blob/main/screenshot/nvidia.png?raw=true"/>

3. 显卡驱动安装完毕后，打开设备管理器，可以查看到显卡信息（如果不安装驱动，是看不到这个的）：

<img width="100%" src="https://github.com/GG4mida/profanity-tron/blob/main/screenshot/gpu.png?raw=true"/>

### 安装 `visual studio`

1. 打开 `visual studio` 官网：[https://visualstudio.microsoft.com/zh-hans/vs/](https://visualstudio.microsoft.com/zh-hans/vs/)

2. 选择以下版本进行下载：

<img width="100%" src="https://github.com/GG4mida/profanity-tron/blob/main/screenshot/vs.png?raw=true"/>

3. 下载后，打开安装程序，安装以下截图所示的组件：

<img width="100%" src="https://github.com/GG4mida/profanity-tron/blob/main/screenshot/vs1.png?raw=true"/>

4. 以上软件安装完成后，就可以直接双击源码目录下面的 `profanity.sln`，打开项目进行开发了。

> 关于 `visual studio` 如何开发、调试、构建 `cpp` 应用程序，不再本文档讨论范围。

### 开发备注

- 不论开发环境是 `windows` 还是 `mac`，在开发调试时可手动指定 `-I` 参数，将其设置为一个较小的值，可极大加快启动速度。
- `mac` 环境可能需要指定 `-w 1`，以生成正确的私钥。
- 部分平台启动异常，可能需要使用 `-s` 参数，跳过设备搭载的集成显卡设备。

## 速度

本程序使用阿里云配置：`GPU 计算型 8 vCPU 32 GiB x 1 * NVIDIA V100`。运行速度在 `2.2亿 H/s` 左右：

<img width="100%" src="https://github.com/GG4mida/profanity-tron/blob/main/screenshot/demo.png?raw=true"/>

> 本程序除了在开发机（一台老旧的 Mac），以及上述 `NVIDIA v100` 显卡上经过测试外，未在其它设备上进行速度测试。

> 请不要纠结于对比各种设备、各种平台差异化的运行速度。没意义。

## 验证

生成的私钥和地址务必进行匹配验证。验证地址：[https://secretscan.org/PrivateKeyTron](https://secretscan.org/PrivateKeyTron)

## 安全

- 本软件基于 [profanity](https://github.com/johguse/profanity) 修改而来，原版程序存在私钥可爆破的漏洞，可参考：[Exploiting the Profanity Flaw](https://medium.com/amber-group/exploiting-the-profanity-flaw-e986576de7ab)

- 本软件已修复原版程序漏洞，详情可查看代码文件：`Dispatcher.cpp` -> `createSeed()`

```cpp
cl_ulong4 Dispatcher::Device::createSeed()
{
#ifdef PROFANITY_DEBUG
	cl_ulong4 r;
	r.s[0] = 1;
	r.s[1] = 1;
	r.s[2] = 1;
	r.s[3] = 1;
	return r;
#else
  // Fix profanity seed create bug, ref: https://medium.com/amber-group/exploiting-the-profanity-flaw-e986576de7ab
	std::random_device rd;
	std::mt19937_64 eng1(rd());
	std::mt19937_64 eng2(rd());
	std::mt19937_64 eng3(rd());
	std::mt19937_64 eng4(rd());
	std::uniform_int_distribution<cl_ulong> distr;

	cl_ulong4 r;
	r.s[0] = distr(eng1);
	r.s[1] = distr(eng2);
	r.s[2] = distr(eng3);
	r.s[3] = distr(eng4);
	return r;
#endif
}
```

## 为什么开源？

- 个人认为这工具其实没什么用，有钱人从来都是朴实无华，不用什么靓号。
- 靠卖软件源码赚不了几个钱，徒耗精力。本人也不靠这个赚钱。
- 还有一些其它原因。

## 打赏

- TRON: TK2yZDdmRXVNPvduHk5BPBeY1KcxUMv4mP

## 一点题外话

现有市面上流传的 `gpu` 类靓号生成程序，基本上都是基于 `profanity` 修改而来。从技术角度来讲，如果出于作恶的目的，完全可以对原版程序的漏洞 `变本加厉`，做到 `秒秒钟` 的私钥爆破。尤其是在不提供源码，仅有一个 `exe 可执行程序` 的情况下，会让作恶的逻辑更加的黑盒。因此再次建议请勿运行任何 `非透明` 的可执行程序，在币圈这种社会达尔文主义盛行的行业，由此导致的资产损失可以说每天都在上演。言尽于此，祝大家好运 🤝

## 联系

- 电报: [@jackslowfak](https://t.me/jackslowfak)