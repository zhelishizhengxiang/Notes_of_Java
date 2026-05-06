## 1. 简介 
### 1.1 什么是 Git LFS 
Git LFS（Large File Storage，大文件存储）是 Git 官方推出的**扩展工具，专门用于解决 Git 不擅长处理大文件（如 `.tar`、`.zip`、`.psd`、`.bin` 等）的问题**。 
### 1.2 解决的核心痛点 
Git 天生不适合存大文件，原因在于：
- Git 会完整保存文件的所有历史版本，大文件会让仓库体积**爆炸式增长** 
- GitHub/Gitee 等平台对单文件大小有严格限制（通常 100MB），超过直接拒绝
- 每次 `clone`/`pull` 都要下载整个大文件，速度极慢 
### 1.3 核心原理 
Git LFS 采用“**指针替换**”的方式： 
1. 大文件**不直接存入 Git 仓库，而是存到独立的 LFS 专用服务器。**    
	![file-20260503143452468.png](assets/02Git%20LFS/file-20260503143452468.png)
	![file-20260503143527159.png](assets/02Git%20LFS/file-20260503143527159.png)
2. Git 仓库里只存一个**几 KB 的“指针文件”**，记录大文件的真实地址 
3. **执行 `clone`/`checkout` 时，Git LFS 自动按需下载真实大文件**
### 1.4 核心优势 
1. 仓库体积小：仅存指针，避免被大文件撑爆 
2. 突破平台限制：可提交超过 100MB 的文件（GitHub 支持最大 2GB/文件）
3. 拉取速度快：不下载大文件完整历史，仅拉取当前版本 
4. **兼容 Git 命令：`commit`/`push`/`pull` 用法和平时一致，几乎无感知** --- 
## 2. 下载、安装与配置 
### 2.1 下载安装 
###### Windows系统 
```bash 
# 方式1：Chocolatey 包管理器安装（推荐） 
choco install git-lfs 
# 方式2：官网手动安装 
# 访问 https://git-lfs.com/ 下载对应版本安装包，双击运行即可
```
###### Mac系统
```bash
# Homebrew 包管理器安装（推荐） 
brew install git-lfs
```
###### Linux系统
```bash
# Ubuntu/Debian 系列
 sudo apt update && sudo apt install git-lfs 
 
 # CentOS/RHEL 系列 
 sudo yum install git-lfs
```

### 2.2 全局初始化配置
**安装完成后，仅需在全局执行一次，即可完成 Git LFS 的全局启用：**
```bash
git lfs install
```
终端输出 `Git LFS initialized.` 即为配置成功。


### 3. 完整操作示例

#### 3.1 核心场景：现有仓库中用 LFS 提交大文件

以实际场景为例：需要提交 `ElasticSearch/archive/es.tar`、`ElasticSearch/archive/kibana.tar` 两个大体积压缩包。
##### 步骤 1：进入目标仓库，初始化仓库级 LFS
每个仓库仅需执行一次，用于启用当前仓库的 LFS 能力：
```bash
# 进入你的仓库根目录
cd /path/to/your/Java_Learning

# 启用当前仓库的 LFS
git lfs install
```
##### 步骤 2：配置 LFS 跟踪规则
告诉 Git LFS 哪些文件需要被它托管，支持通配符、指定目录、单文件三种方式：
```bash
# 方式1：跟踪所有 .tar 结尾的文件（推荐，一劳永逸）
git lfs track "*.tar"

# 方式2：仅跟踪指定目录下的 .tar 文件
git lfs track "Elasticsearch/archive/*.tar"

# 方式3：仅跟踪单个指定文件
git lfs track "Elasticsearch/archive/es.tar"
```

**执行后会自动在仓库根目录生成 `.gitattributes` 文件，该文件记录了 LFS 的所有跟踪规则，必须和代码一起提交到仓库，否则团队成员无法正常拉取 LFS 文件***。
##### 步骤 3：添加文件并提交
```bash
# 1. 必须先添加 .gitattributes 规则文件
git add .gitattributes

# 2. 添加需要提交的大文件
git lfs add Elasticsearch/archive/es.tar Elasticsearch/archive/kibana.tar

# 3. 执行提交
git commit -m "feat: 新增 ES、Kibana 安装包，通过 Git LFS 托管"
```
##### 步骤 4：推送到远程仓库

```bash
git push
```
**执行后，Git 会自动将大文件上传到远程仓库的 LFS 存储服务器，Git 仓库仅提交指针文件，不会触发「文件过大」的报错**。
### 3.2 常用场景补充示例

#### 场景 1：克隆包含 LFS 文件的仓库
```bash
# 完整克隆，自动下载所有 LFS 对应的真实文件
git clone https://github.com/你的用户名/你的仓库名.git

# 浅克隆，仅下载最新版本的 LFS 文件，大幅提升克隆速度
git clone --depth 1 https://github.com/你的用户名/你的仓库名.git
```
如果克隆后大文件仅显示为指针文本，手动执行拉取命令即可：

```bash
git lfs pull
```
#### 场景 2：查看与修改 LFS 跟踪规则

```bash
# 重置当前仓库所有 LFS 跟踪规则
git lfs track

# 取消对某类文件的跟踪
git lfs untrack "*.tar"

# 查看当前仓库 LFS 文件的状态
git lfs status
```
#### 场景 3：清理本地 LFS 缓存
Git LFS 会在本地缓存已下载的大文件，缓存占用过大时可执行清理：
```bash
# 清理未被当前分支引用的 LFS 缓存文件
git lfs prune
```
### 4. 操作核心注意细节

1. **必须提交 `.gitattributes` 文件**
   该文件是 LFS 跟踪规则的核心，未提交会导致：团队成员拉取代码后，大文件仅显示为指针文本，无法正常使用；新提交的同类型大文件不会被 LFS 托管，仍会触发文件过大报错。
2. **禁止手动修改指针文件**
   LFS 托管的文件在 Git 仓库中会显示为指针文本，包含版本、哈希、大小等信息，手动修改会导致文件损坏，无法正常拉取到真实文件。
3. **不要混用普通 Git 提交与 LFS 提交**
   已经被 LFS 跟踪的文件类型，不要用原生 Git 强制提交，否则会导致仓库同时存在指针文件和真实大文件，仓库体积反而膨胀，后续清理难度极大。
4. **先配置跟踪规则，再提交大文件**
   必须先执行 `git lfs track` 配置规则，再添加大文件提交；如果先提交了大文件，再配置规则，已经提交到 Git 历史的大文件不会被自动迁移到 LFS，需要手动执行迁移命令。git
5. **避免频繁修改大文件**
   大文件每修改一次，LFS 服务器就会留存一个新的完整版本，会快速占用存储额度，建议仅提交稳定的大文件版本，减少无意义的修改提交。

### 5. 补充注意事项

#### 5.1 主流平台 LFS 免费额度与限制

不同代码托管平台的 LFS 免费存储、带宽额度不同，超出后需付费升级，具体如下：

| 托管平台   | 免费存储额度 | 免费月带宽额度  | 单文件最大限制 |
| :----- | :----- | :------- | :------ |
| GitHub | 10GB   | 10GB / 月 | 2GB     |
| Gitee  | 5GB    | 5GB / 月  | 2GB     |
| GitLab | 10GB   | 10GB / 月 | 无明确限制   |

> ⚠️ 关键提醒：带宽额度指的是「他人从 LFS 服务器下载文件的流量」，如果仓库被大量克隆、下载，会快速耗尽免费额度，超出后平台会直接禁止 LFS 文件的下载。

#### 5.2 超出额度的付费成本

如果免费额度不足，各平台主流付费方案如下（2026 年最新参考）：
- **GitHub**：5 美元 / 月，新增 50GB 存储 + 50GB 月带宽
- **Gitee**：9.9 元 / 月，新增 50GB 存储 + 50GB 月带宽
- **GitLab**：4 美元 / 月 / 用户，新增 10GB 存储额度
#### 5.3 历史大文件迁移到 LFS

**如果仓库历史中已经用原生 Git 提交了大文件，可通过 `git lfs migrate` 命令批量迁移到 LFS，彻底清理仓库历史的大文件**：
```bash
# 迁移仓库历史中所有 .tar 结尾的文件到 LFS
git lfs migrate import --include="*.tar" --everything

# 强制推送到远端仓库（会覆盖提交历史，团队协作需提前同步所有人）
git push --force-with-lease
```
#### 5.4 团队协作注意事项
- 所有参与项目的成员，**必须安装 Git LFS**，否则拉取代码后无法获取到真实的大文件，仅能看到指针文本
- 建议在仓库的 `README.md` 中明确标注「本项目使用 Git LFS 管理大文件，请先安装 Git LFS 后再克隆」
- 统一团队的 LFS 跟踪规则，避免不同成员各自配置规则，导致大文件管理混乱
 

#### 5.5 备份与安全
- LFS 服务器中的大文件，**不会被 Git 仓库的常规备份自动包含**，建议定期将重要大文件备份到本地硬盘或私有网盘
- 敏感大文件不建议仅托管在公有平台的 LFS 服务器，建议搭配私有 LFS 服务器或加密存储使用


### 6. 最佳实践总结
1. **非必要不提交大文件**：对于笔记仓库、文档项目，优先将压缩包、安装包等资源文件加入 `.gitignore`，仅在笔记中记录本地路径或网盘链接，无需提交到 Git
2. **必须提交的大文件，优先使用 Git LFS**：不要用原生 Git 强制提交大文件，避免仓库体积膨胀、远程仓库拒绝提交
3. **提前规划跟踪规则**：项目初始化时就配置好 `.gitattributes`，统一管理需要 LFS 托管的文件类型
4. **控制大文件修改频率**：减少大文件的无意义修改，避免快速耗尽存储与带宽额度