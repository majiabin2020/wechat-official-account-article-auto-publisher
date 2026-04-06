# Contributing

欢迎提交 Issue 和 Pull Request。

## 开发建议

- 保持命令行输出为 JSON
- 新功能优先放进 `scripts/wechat_skill/`，避免把逻辑堆回单入口脚本
- 修改创作逻辑时，同时更新：
  - `CREATION_GUIDE.md`
  - `SKILL.md`
  - `templates/`

## 提交前检查

1. 安装依赖
   - `python scripts/publish_wechat.py install`
2. 运行关键命令帮助
   - `python scripts/publish_wechat.py --help`
   - `python scripts/publish_wechat.py workflow --help`
3. 运行基本语法检查
   - `python -m py_compile scripts/publish_wechat.py scripts/wechat_skill/*.py`

## 提交规范

- 提交信息尽量简洁明确
- 一个 PR 聚焦一个主题
- 涉及 CLI、文档或模板的改动，尽量配上示例命令
