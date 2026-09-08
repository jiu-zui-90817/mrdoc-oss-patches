# 安装：无限文档层级

详见 [META.md](./META.md)。

简要步骤（在 MrDoc 根目录）：

```bash
patch -p1 < patches/unlimited-doc-level/diffs/views.py.patch
patch -p1 < patches/unlimited-doc-level/diffs/create_doc.html.patch
patch -p1 < patches/unlimited-doc-level/diffs/modify_doc.html.patch
docker restart mrdoc
```

适用上游：Gitee `zmister/MrDoc` master @ 2026-09-08（v1.1.0 代码树）。
