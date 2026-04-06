# 微信公众号文章自动发布助手

`微信公众号文章自动发布助手` 是一个面向 Skill 场景的微信公众号文章工作流工具。它支持文章智能创作、参考文章改写、公众号文章提取、封面生成、素材上传、草稿创建与发布，并提供从“一句话需求”到“初稿预览”的完整命令行流程。

英文标识：

- `wechat-official-account-article-auto-publisher`

## 功能概览

- 基于标题直接生成 1200-1500 字公众号文章
- 基于参考文章或 `mp.weixin.qq.com` 链接改写文章
- 提取网页或微信公众号文章并转换为 Markdown
- 生成封面图，支持豆包和通义千问
- 上传图片等素材到微信服务器
- 创建公众号草稿并按需发布
- 一条命令完成 `create -> write -> preview -> draft`

## 目录结构

```text
agents/
scripts/
templates/
config.json
CREATION_GUIDE.md
SKILL.md
_meta.json
```

关键文件：

- [SKILL.md](./SKILL.md)：Skill 使用说明
- [config.json](./config.json)：运行配置
- [scripts/publish_wechat.py](./scripts/publish_wechat.py)：主命令入口
- [scripts/wechat_skill](./scripts/wechat_skill)：核心实现
- [templates](./templates)：创作提示、对话模板和示例稿

## 环境要求

- Python 3.11 或更高版本
- Windows、macOS、Linux 均可运行
- 如需真实创建公众号草稿，需要微信公众号 `AppID` 与 `AppSecret`
- 如需自动生成封面，需要配置豆包或通义千问图像接口

## 安装依赖

```bash
python scripts/publish_wechat.py install
```

依赖列表见 [scripts/requirements.txt](./scripts/requirements.txt)。

## 配置说明

编辑 [config.json](./config.json)：

```json
{
  "wechat": {
    "app_id": "",
    "app_secret": "",
    "author": "",
    "default_template": "business"
  },
  "image_generation": {
    "provider": "doubao",
    "doubao": {
      "api_key": "",
      "model": "",
      "base_url": "https://ark.cn-beijing.volces.com/api/v3"
    },
    "qwen": {
      "api_key": "",
      "model": "wanx2.1-t2i-turbo",
      "base_url": "https://dashscope.aliyuncs.com/api/v1"
    }
  }
}
```

说明：

- `wechat.app_id` / `wechat.app_secret`：用户必须自行填写
- `image_generation.doubao.api_key`：用户必须自行填写
- `image_generation.doubao.model`：用户必须自行填写
- `image_generation.qwen.api_key`：用户必须自行填写
- `image_generation.qwen.model`：已提供默认值，但仍建议按自己的账号能力确认

## 快速开始

### 1. 基于一句话需求生成初稿与预览

```bash
python scripts/publish_wechat.py workflow "AI 产品经理的第二曲线" --request "写给产品经理，分析 AI 为什么会先压缩中间层价值，并给出转型建议" --dry-run
```

### 2. 提取公众号文章为 Markdown

```bash
python scripts/publish_wechat.py extract "https://mp.weixin.qq.com/s/xxxxx"
```

### 3. 基于创作资产生成文章初稿

```bash
python scripts/publish_wechat.py create "AI 产品经理的第二曲线" --request "写给产品经理，分析 AI 为什么会先压缩中间层价值，并给出转型建议"
python scripts/publish_wechat.py write outputs/ai-产品经理的第二曲线-creation
```

### 4. 创建公众号草稿

```bash
python scripts/publish_wechat.py draft article.md --template business
```

### 5. 创建草稿并直接发布

```bash
python scripts/publish_wechat.py draft article.md --publish --status
```

## CLI 子命令

- `install`：安装依赖
- `create`：生成创作资产
- `write`：基于创作资产生成初稿并自动质检
- `workflow`：一条命令完成 create、write、preview，并按需创建草稿
- `extract`：提取 URL 或本地 Markdown
- `cover`：生成封面
- `material`：上传素材
- `draft`：创建草稿
- `publish`：发布已有草稿

查看帮助：

```bash
python scripts/publish_wechat.py --help
python scripts/publish_wechat.py workflow --help
```

## 产物说明

默认运行产物会写到 `outputs/` 目录，包括：

- `brief.json`
- `prompt.md`
- `outline.md`
- `article.md`
- `generated_check.json`
- `*-wechat-preview.html`

这些文件是运行产物，不属于仓库源码。

## 开发与调试

- 主要逻辑在 [scripts/wechat_skill](./scripts/wechat_skill)
- 创作规范见 [CREATION_GUIDE.md](./CREATION_GUIDE.md)
- Skill 使用说明见 [SKILL.md](./SKILL.md)

语法检查：

```bash
python -m py_compile scripts/publish_wechat.py scripts/wechat_skill/*.py
```

## 发布到 GitHub 前建议

- 保持 `config.json` 中敏感字段为空
- 不要提交 `outputs/`
- 不要提交 `dist/`
- 不要提交 `__pycache__/`

## License

[MIT](./LICENSE)
