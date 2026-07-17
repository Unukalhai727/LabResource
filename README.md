# LabResource

LabResource 是 HDU SIIG Lab 的实验室资源文档站点，使用 [MkDocs](https://www.mkdocs.org/) 构建。项目集中记录实验室服务器、数据集及日常运维流程，方便成员查询资源和维护环境。

当前文档主要包括：

- 用户手册：服务器配置、访问方式、安全策略和数据集存放位置；
- 运维手册：Ubuntu Server 重置、用户与权限管理、NAS 自动挂载等操作指南。

## 项目结构

```text
.
├── docs/                # Markdown 文档源文件
│   ├── index.md         # 站点首页
│   ├── user/            # 用户手册
│   └── admin/           # 运维手册
└── mkdocs.yml           # MkDocs 站点及导航配置
```

## 本地预览

请先安装 Python 3 和 MkDocs。建议使用虚拟环境隔离依赖：

```bash
python -m venv .venv
```

激活虚拟环境：

```bash
# Linux / macOS
source .venv/bin/activate

# Windows PowerShell
.venv\Scripts\Activate.ps1
```

安装 MkDocs：

```bash
python -m pip install mkdocs
```

在项目根目录启动开发服务器：

```bash
mkdocs serve
```

浏览器访问 <http://127.0.0.1:8000/> 即可预览。修改 `docs/` 下的文件后，页面会自动刷新。

## 构建与部署

### 构建静态站点

```bash
mkdocs build --strict
```

构建结果默认输出到 `site/` 目录。部署时只需将该目录中的文件发布到任意静态网站服务。

### 部署到 GitHub Pages

确保本地仓库已配置 GitHub 远程地址，并且当前账号具有推送权限，然后执行：

```bash
mkdocs gh-deploy --clean
```

该命令会构建站点并将结果推送到远程仓库的 `gh-pages` 分支。随后在仓库的 **Settings → Pages** 中选择从 `gh-pages` 分支发布。

## 更新文档

1. 在 `docs/` 下新增或修改 Markdown 文件；
2. 如需新增页面，在 `mkdocs.yml` 的 `nav` 中补充导航项；
3. 使用 `mkdocs serve` 本地检查内容；
4. 使用 `mkdocs build --strict` 确认站点可以正常构建后再部署。

> 文档可能包含实验室内部资源信息。公开部署前，请确认服务器地址、端口和存储路径等内容符合实验室的信息公开与安全要求。
