# picx-images-hosting-4

图澜 Image API Platform 的图片存储仓库（**4 仓库分片之 4**）。所有图片统一为 **WebP** 格式，文件名即内容 SHA-256（天然去重），通过 jsDelivr CDN 对外提供访问。

## 分片架构

平台共使用 4 个图床仓库分片存储（单仓库 5GB 上限，达 90% 自动跳过）：

| # | 仓库 |
|---|---|
| 1 | [IIXINGCHEN/picx-images-hosting](https://github.com/IIXINGCHEN/picx-images-hosting) |
| 2 | [IIXINGCHEN/picx-images-hosting-2](https://github.com/IIXINGCHEN/picx-images-hosting-2) |
| 3 | [IIXINGCHEN/picx-images-hosting-3](https://github.com/IIXINGCHEN/picx-images-hosting-3) |
| 4 | [IIXINGCHEN/picx-images-hosting-4](https://github.com/IIXINGCHEN/picx-images-hosting-4) |

新图片上传时平台自动选择剩余空间最大的分片写入；D1 数据库以 SHA-256 全局去重，同一张图不会在多个分片重复存储。

## 目录结构

按**内容**组织（英文小写扁平命名），来源信息由平台数据库维护：

| 目录 | 说明 |
|---|---|
| `anime/` | 动漫（含二次元、游戏、虚拟主播、角色） |
| `scenery/` | 风景、星空 |
| `portrait/` | 人像 |
| `pets/` | 萌宠 |
| `misc/` | 待分类 / 其他 |

## 访问方式

通过 jsDelivr（`@master` 分支）：

```
https://cdn.jsdelivr.net/gh/IIXINGCHEN/picx-images-hosting-4@master/<目录>/<sha256>.webp
```

示例：

```
https://cdn.jsdelivr.net/gh/IIXINGCHEN/picx-images-hosting-4@master/anime/xxx.webp
```

## 图片规范

- 格式：WebP（quality 85）
- 命名：`{sha256}.webp`，相同内容只存一份
- 元数据（尺寸、分类、标签、来源、分片位置）由图澜平台 D1 数据库维护，本仓库只存图片文件

## 自动采集

图片由图澜采集管线自动拉取、去重、转码后提交到本仓库（commit 信息以 `ingest:` 开头）。请勿手动上传重名文件；如需删除图片，请同步通知平台方删除 D1 中的索引记录。
