# diffs 说明

本目录为 **unified diff**，在 MrDoc 代码根目录执行：

```bash
patch -p1 < diffs/create_doc.html.patch
patch -p1 < diffs/modify_doc.html.patch
patch -p1 < diffs/views.py.patch
```

## 注意：`views.py.patch`

若 `views.py.patch` 与你本地官方 `views.py` 对不齐，请改用**完整替换文件**：

- 使用已验证可运行的完整 `app_doc/views.py`（见本补丁 `files/app_doc/views.py`，若尚未入库则以你生产环境已生效的版本为准并备份上传）

前端两个 patch 体积小、冲突少，优先用 patch 方式。
