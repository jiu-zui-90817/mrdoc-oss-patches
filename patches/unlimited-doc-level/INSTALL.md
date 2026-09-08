# 安装：无限文档层级

1. 确认上游为 Gitee MrDoc `master`（约 2026-09-08 / v1.1.0 代码树）。详见 `META.md`。
2. 备份生产环境三个文件。
3. 用本补丁目录内同路径文件覆盖：
   - `app_doc/views.py`
   - `template/app_doc/editor/create_doc.html`
   - `template/app_doc/editor/modify_doc.html`
4. `docker restart mrdoc`，浏览器强制刷新。
