# fydeRhythm-builder

fydeRhythm 的构建脚本，参考内容包括：

- fydeRhythm Wiki: [https://github.com/FydeOS/fydeRhythm/wiki/%E9%A1%B9%E7%9B%AE%E6%9E%84%E5%BB%BA%E6%B5%81%E7%A8%8B](https://github.com/FydeOS/fydeRhythm/wiki/%E9%A1%B9%E7%9B%AE%E6%9E%84%E5%BB%BA%E6%B5%81%E7%A8%8B)

## fydeRhythm编译

## librime-wasm编译步骤

**本部分内容暂未成功**，卡在glog。

### 1. 安装并配置 Emscripten

```bash
# 克隆 emsdk
git clone https://github.com/emscripten-core/emsdk.git
cd emsdk

# 安装并激活 Emscripten 3.1.31，按照wiki要求用特定版本，不然有各种问题
./emsdk install 3.1.31
./emsdk activate 3.1.31

# 设置环境变量
source emsdk_env.sh
```

### 2. 克隆 librime-wasm项目

```bash
cd ..
git clone https://github.com/FydeOS/librime-wasm.git
```

### 3. 构建 RIME 依赖

进入 librime-wasm 目录，下载并编译依赖：

```bash
cd librime-wasm/wasm-builder/dep

# 下载代码并编译所有依赖 (boost, yaml-cpp, marisa, opencc, glog)
./build-all
```

### 4. 下载 RIME 插件并构建 RIME WebAssembly

```bash
cd ../

./download-plugins
./build-wasm
```

### 5. 检查构建结果

主要构建结果包括：

- `rime_emscripten.js`: 需要放到 fydeRhythm 的 `background/`
- `rime_emscripten.wasm`: 需要放到 fydeRhythm 的 `assets/`

### 编译问题

- 编译 marisa-trie 时需要修改 `build-marisa` 脚本，移除 `--enable-native-code`
- glog 版本问题，0.6.0的glog不支持Emscripten，但是向前找第一个加入Emscripten的Commit也不行（进入0.7.0版本，API改变）
- 如果 Boost 下载失败，修改 `wasm-builder/dep/fetch-source` 中的Boost下载地址：`https://sf-west-interserver-2.dl.sourceforge.net/project/boost/boost/1.77.0/boost_1_77_0.tar.gz boost-1.77.0.tar.gz`