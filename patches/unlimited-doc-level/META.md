# 补丁：无限文档层级 (unlimited-doc-level)

## 适用上游版本

- **仓库**：https://gitee.com/zmister/MrDoc
- **分支**：`master`
- **快照日期**：2026-09-08
- **版本说明**：上游 CHANGES 已出现 **v1.1.0** 相关记录；Release 标签当时常见仍为 **0.9.9**。以 **master 代码结构**为准。
- **验证环境**：Docker 官方运行镜像 + 挂载代码；已验证可选第 4 级及以上上级文档。

若你的代码与该日期 master 差异较大，请先 `diff` 再合并，不要盲覆盖。

## 功能说明

开源版默认最多 3 级文档。本补丁：

1. 后端目录树 / 排序 / 选上级 / 文档树 API 改为递归，支持任意深度
2. 前端新建、修改、移动文档时去掉「第三级文档不能作为上级文档」拦截

## 推荐安装方式：应用 unified diff（更易合并）

在 **MrDoc 代码根目录**（含 `app_doc/`、`template/` 的目录）执行：

```bash
# 备份
cp app_doc/views.py app_doc/views.py.bak
cp template/app_doc/editor/create_doc.html template/app_doc/editor/create_doc.html.bak
cp template/app_doc/editor/modify_doc.html template/app_doc/editor/modify_doc.html.bak

# 应用补丁（将 PATH 换成本仓库中 diffs 目录）
patch -p1 < PATH/to/diffs/views.py.patch
patch -p1 < PATH/to/diffs/create_doc.html.patch
patch -p1 < PATH/to/diffs/modify_doc.html.patch

docker restart mrdoc
# 浏览器 Ctrl+F5
```

若 `patch` 失败，说明官方已改同一段代码，需手工合并：对照 `diffs/*.patch` 中的改动点。

## 备选：完整文件覆盖

目录 `files/` 下若提供与 MrDoc 相同相对路径的完整文件，可直接覆盖（冲突风险更高）。

| 相对路径 |
|----------|
| `app_doc/views.py` |
| `template/app_doc/editor/create_doc.html` |
| `template/app_doc/editor/modify_doc.html` |

## 官方升级后

1. 官方 `git pull` 之后再打一遍 patch
2. 关注函数：`get_pro_toc`、`get_pro_doc`、`manage_project_doc_sort`、`get_pro_doc_tree`
3. 前端搜索：`第三级文档不能作为上级文档`、`level != 3`

## 变更记录

| 日期 | 说明 |
|------|------|
| 2026-09-08 | 首版：后端递归 + 前端取消三级限制；入库 unified diff |
