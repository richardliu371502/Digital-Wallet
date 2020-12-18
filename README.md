# 雄安链应用系统接入指南（SDK使用）
---
本文将介绍如何接入雄安区块链，以帮助读者理解搭建运行环境、部署智能合约和合约编译、运行的整个流程。

本教程以Ubuntu 16.04 版本为例，go语言版本教程采用版本为1.14.4。因兼容性问题，推荐本系统使用1.14版本的golang语言环境
## 介质获取
* xccli：client命令行工具；
* xchain-java-sdk-1.0-SNAPSHOT.jar  该包为对应的Java SDK包
* 合约Demo
* SDK Demo

---
## Rust智能合约开发环境搭建
```
接入雄安链应用系统应先具有Rust智能合约安装环境，具体安装步骤如下所示：
1 按照官方安装go环境，本教程推荐使用1.14.4版本。安装好后，在命令行输入go version验证版本

如上图所示， golang1.14.4版本运行环境已经安装成功

2 装rustup，rustup是rust的安装和管理工具，在本项目中rustup用来维护依赖和处理cargo、rustc的版本更新，安装脚本：
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
rustup default stable
刷新环境变量：source $HOME/.cargo/env
cargo version
如上图所示，cargo的版本为
如果版本低于1.44.1，用下面命令更新：
rustup update stable
此命令将会更新本地的rustup版本。
rust安装成功后，通过下面命令，确认安装了wasm32 target
rustup target list --installed
如果显示并没有安装wasm32的话，执行以下命令：
rustup target add wasm32-unknown-unknown
再次执行rustup target list --installed，可以看到已经安装成功/ 
3 选择开发智能合约的ide，使用vscode安装rust插件或idea安装intellij-rust和intellij-toml以便更好的开发智能合约。 intellij-rust是Rust语言插件，intellij-toml是为Toml语言的插件。.本教程使用Intellij IDEA为例：
* 安装Intellij IDEA，可以根据自己的需要选择商业版或免费版。
* 安装开发智能合约需要的两个插件rust和toml。安装方式：Perferences.. -> Plugins 在Marketplact中直接搜索Rust和toml并安装即可
* 合约开发请参考Demo来完成。 
```
## 通过xccli接入区块链网络环境
```
1 将xccli文件所在目录加入path环境变量
2 通过xccli生成一个组织管理员秘钥对和地址
命令行输入xccli keys add <管理员账号名>，如输入xccli keys add admin，则生成名为admin的管理员，输入命令后命令行信息如下文所示：
 {
  "name": "admin",
  "type": "local",
  "address": "cosmos1u2aqzefhj37e7k9akercv80gfxauwjayk77gez",
  "pubkey": "cosmospub1addwnpepqfn7s74hhmyqkqj09xvvttuezp6qe8d27yxa3fksn3emfenczr6acmq2v59",
  "mnemonic": "all cage select bench blanket chapter capable artefact other alone toss ghost need fall dinner rapid rather control budget alpha solid dial name special"
}
其中mnemonic为管理员的私钥的助记符。
3 在命令行输入xccli status，获取网络名称、channel、rpc_address等信息

如上图所示，我们已经可以看到网络名称和rpc_address等相关信息。
4 把管理员地址等信息发给网络管理员。 
```
## 智能合约开发
```
基于合约Demo，进行rust语言合约开发，Demo为idea的rust工程。
合约编译：
* 进入到合约目录
* 进入到合约目录后，在命令行运行RUSTFLAGS='-C link-arg=-s' cargo wasm。根据网络情况稍等一段时间后，合约编译完成结果如下所示。 
* 如果合约编译非常慢，需要更换cargo 依赖源：
1、进入.cargo目录，默认在当前用户目录下.
2、创建一个config文件，写入以下内容：
[source.crates-io]
registry = "https://github.com/rust-lang/crates.io-index"
replace-with = 'ustc'
[source.ustc]
registry = "git://mirrors.ustc.edu.cn/crates.io-index"
* 删除.package-cache文件
* 编译完毕的合约，默认在target/wasm32-unknown-unknown/release目录下，扩展名wasm
---

```
