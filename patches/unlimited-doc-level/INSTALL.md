# 安装说明（两种方式）

## 方式 A：完整文件替换（当前已验证）

从本补丁的 `files/` 目录（若已提供）或项目会话产物中，覆盖：

- `app_doc/views.py`
- `template/app_doc/editor/create_doc.html`
- `template/app_doc/editor/modify_doc.html`

然后 `docker restart mrdoc`，浏览器 Ctrl+F5。

## 方式 B：应用 unified diff（适合升级后合并）

在 MrDoc 代码根目录：

```bash
patch -p1 < patches路径/diffs/app_doc_views.py.patch
patch -p1 < patches路径/diffs/create_doc.html.patch
patch -p1 < patches路径/diffs/modify_doc.html.patch
```

若 `patch` 报冲突，用 `git apply --3way` 或手动对照 `META.md` 中的函数名合并。
