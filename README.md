# MrDoc 开源版补丁仓库

针对 [Gitee zmister/MrDoc](https://gitee.com/zmister/MrDoc) 开源版的补丁集合。

**用法很简单：每个补丁目录里的文件路径与官方仓库一致，复制覆盖到你挂载的 MrDoc 代码目录即可。**

## 补丁列表

| 补丁 | 目录 | 适用上游 |
|------|------|----------|
| 无限文档层级 | [patches/unlimited-doc-level/](patches/unlimited-doc-level/) | Gitee `master` @ 2026-09-08（CHANGES 含 v1.1.0；Release 标签常见仍为 0.9.x） |

## 通用安装（Docker 官方镜像）

```bash
# 假设你的代码挂载在 /opt/MrDoc
cd /opt/MrDoc

# 备份
cp app_doc/views.py app_doc/views.py.bak
cp template/app_doc/editor/create_doc.html template/app_doc/editor/create_doc.html.bak
cp template/app_doc/editor/modify_doc.html template/app_doc/editor/modify_doc.html.bak

# 覆盖（把下面源路径换成本仓库 patches/unlimited-doc-level）
cp -a patches/unlimited-doc-level/app_doc/views.py app_doc/views.py
cp -a patches/unlimited-doc-level/template/app_doc/editor/create_doc.html template/app_doc/editor/create_doc.html
cp -a patches/unlimited-doc-level/template/app_doc/editor/modify_doc.html template/app_doc/editor/modify_doc.html

docker restart mrdoc
# 浏览器 Ctrl+F5
```

每个补丁目录内的 `META.md` 写明适用版本与实现说明。官方升级后请对照 META 再覆盖或手工合并。
