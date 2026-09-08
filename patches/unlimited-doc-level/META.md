# 补丁：无限文档层级 (unlimited-doc-level)

## 适用上游版本

- **仓库**：https://gitee.com/zmister/MrDoc
- **分支**：`master`
- **快照日期**：2026-09-08
- **版本说明**：上游 CHANGES 已出现 **v1.1.0** 相关记录；GitHub/Gitee **Release 标签**当时仍常见为 **0.9.9** 一带。以 **master 代码结构**为准，而非仅看 tag。
- **验证环境**：Docker 官方运行镜像 + 宿主机挂载代码目录；生产已验证可选第 4 级及以上上级文档。

若你的代码与 2026-09-08 的 master 差异较大，请先 diff 再合并，不要直接覆盖。

## 功能说明

开源版默认最多 3 级文档。本补丁：

1. 后端目录树 / 排序 / 选上级 / 文档树 API 改为递归，支持任意深度
2. 前端新建、修改、移动文档时去掉「第三级文档不能作为上级文档」拦截

## 涉及文件（替换清单）

| 补丁内路径 | 覆盖到你挂载目录中的路径 |
|------------|--------------------------|
| `app_doc/views.py` | `app_doc/views.py` |
| `template/app_doc/editor/create_doc.html` | `template/app_doc/editor/create_doc.html` |
| `template/app_doc/editor/modify_doc.html` | `template/app_doc/editor/modify_doc.html` |

完整可替换文件位于上述路径（本目录下保持与 MrDoc 相同的相对路径）。

## 安装步骤

```bash
# 1. 备份
cp app_doc/views.py app_doc/views.py.bak
cp template/app_doc/editor/create_doc.html template/app_doc/editor/create_doc.html.bak
cp template/app_doc/editor/modify_doc.html template/app_doc/editor/modify_doc.html.bak

# 2. 用本目录三个文件覆盖上述路径

# 3. 重启
docker restart mrdoc

# 4. 浏览器 Ctrl+F5 强刷
```

## 自测建议

- 连续创建第 4、5 级文档
- 左侧大纲是否完整显示
- 编辑页选择三级文档作为上级是否成功
- 文档排序拖拽是否正常

## 官方升级后的合并提示

- `views.py`：关注 `get_pro_toc`、`get_pro_doc`、`manage_project_doc_sort`、`get_pro_doc_tree` 是否被官方重写
- 两个 html：搜索 `第三级文档不能作为上级文档` 或 `level != 3`，官方若仍有则需再次去掉

## 变更记录

| 日期 | 说明 |
|------|------|
| 2026-09-08 | 首版：后端递归 + 前端取消三级限制；完整文件入库 |
