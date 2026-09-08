# 补丁：无限文档层级

## 适用上游版本

- 仓库：https://gitee.com/zmister/MrDoc
- 分支：`master`
- 快照：2026-09-08
- 说明：上游 CHANGES 已有 **v1.1.0**；Release 标签当时常见为 **0.9.9**。以 master 代码为准，不要只看 tag。
- 验证：Docker 官方运行镜像 + 宿主机挂载代码；可选第 4 级及以上上级文档。

若你的代码与上述日期差异大，先 diff 再覆盖。

## 覆盖文件（路径与官方一致）

```
app_doc/views.py
template/app_doc/editor/create_doc.html
template/app_doc/editor/modify_doc.html
```

本目录下已按相同相对路径放好，直接 `cp` 到 MrDoc 根目录对应位置即可。

## 实现说明

开源版「最多 3 级」来自两处，不是数据库限制：

### 1. 后端 `app_doc/views.py`

官方把目录树写死为「一级 → 二级 → 三级」循环查询。本补丁改为按 `parent_doc` 建 `children_map` 后**递归**生成树。

改动的函数：

| 函数 | 作用 |
|------|------|
| `get_pro_toc` | 文集大纲 / 目录数据，递归 `sub` |
| `get_pro_doc` | 选上级文档列表；去掉「只要二级目录」过滤，返回全部文档并带路径前缀 |
| `manage_project_doc_sort` | 排序页树与保存；递归处理任意深度 `children` |
| `get_pro_doc_tree` | 文档树 API；递归 `children`，带 `level` |

未改数据库模型，`parent_doc` 仍是整型上级 ID。

### 2. 前端两个编辑页模板

官方 JS 里写了 `if (obj.data.level != 3)`，否则提示「第三级文档不能作为上级文档」。本补丁去掉该判断（修改页仍禁止选自己）。

- `create_doc.html`：新建时点选上级
- `modify_doc.html`：修改时点选上级 + 移动文档时点选上级

### 为何之前只改后端不够

后端放开后，前端仍会弹出三级限制提示，必须两处一起改。

## 安装

```bash
cd /你的/MrDoc根目录
cp app_doc/views.py app_doc/views.py.bak
cp template/app_doc/editor/create_doc.html template/app_doc/editor/create_doc.html.bak
cp template/app_doc/editor/modify_doc.html template/app_doc/editor/modify_doc.html.bak

cp 本补丁目录/app_doc/views.py app_doc/views.py
cp 本补丁目录/template/app_doc/editor/create_doc.html template/app_doc/editor/create_doc.html
cp 本补丁目录/template/app_doc/editor/modify_doc.html template/app_doc/editor/modify_doc.html

docker restart mrdoc
```

## 官方升级后

1. 官方更新后这三个文件可能被覆盖
2. 再执行一次覆盖，或对 `views.py` 用 diff 合并递归逻辑
3. 前端搜索 `第三级文档不能作为上级文档` / `level != 3`，有则再删

## 变更记录

| 日期 | 说明 |
|------|------|
| 2026-09-08 | 首版：后端递归 + 前端取消三级限制 |
| 2026-09-09 | 整理为与官方同路径可直接覆盖的完整文件包 |
