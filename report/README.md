\# 实践项目2：个人实验报告



\## 1. 基本信息

\- GitHub 登录名：sakgratama-collab

\- 个人仓库地址：https://github.com/sakgratama-collab/my-docs

\- 最终网站地址：https://sakgratama-collab.github.io/my-docs/



\## 2. 实践过程与关键截图



\### 2.1 仓库创建

在本人账号下创建公开仓库 my-docs，勾选 README 初始化，默认分支为 main，随后克隆到本地 D:\\code\\my-docs。

!\[个人仓库](images/01-repository.png)



\### 2.2 项目结构与 MkDocs 配置

docs/ 目录存放网站源内容（四个 Markdown 页面）；mkdocs.yml 的 nav 定义导航栏页面顺序，路径相对于 docs/；site/ 是 MkDocs 构建生成的成品网页，由机器自动产生，因此写入 .gitignore 不提交，每次部署时由 GitHub Actions 现场重新构建。

!\[项目结构](images/02-structure.png)



\### 2.3 本地构建或预览

用 py -3.12 创建虚拟环境并安装 requirements.txt 依赖，python -m mkdocs build --strict 构建无警告，mkdocs serve 本地预览 127.0.0.1:8000，逐页检查了四页导航、表格边框、代码高亮和页面间链接均正常。

!\[本地预览](images/03-local-preview.png)



\### 2.4 Pages 设置

在 Settings → Pages 中将发布来源设置为 GitHub Actions，未使用分支发布方式。

!\[Pages设置](images/04-pages-setting.png)



\### 2.5 Actions 首次构建和部署

推送后工作流自动触发，build 与 deploy 两个任务均成功。

!\[Actions成功](images/05-actions-success.png)



\### 2.6 网站访问与四页导航检查

通过 https://sakgratama-collab.github.io/my-docs/ 访问网站，首页、关于、使用指南、API 参考四页均正常打开，导航与链接可用。

!\[网站效果](images/06-website.png)



\### 2.7 第二次推送与自动更新验证

在 docs/index.md 末尾新增一句验证文字，仅执行 add/commit/push，未手动触发，工作流自动运行第二次，成功后网站出现新增文字。

\- 对应提交链接：https://github.com/sakgratama-collab/my-docs/commit/e58b5c8306ec6c9f38765f3564de8f68c7c1d97b

\- 对应 Actions 运行链接：https://github.com/sakgratama-collab/my-docs/actions/runs/41787817217

!\[第二次运行](images/07-second-run.png)

!\[网站更新](images/07-website-updated.png)



\## 3. 问题与解决

本次部署过程没有遇到构建或部署失败。为确保正确，我核对过：mkdocs.yml 的 nav 路径是否相对于 docs/、YAML 是否使用空格缩进、.gitignore 是否挡住 site/ 和 .venv/、git status 中暂存内容是否不含生成文件、Actions 日志中 build 与 deploy 是否均为绿色。此前作业中我踩过的坑对本次有帮助：本机 python 命令会命中 Microsoft Store 存根，所以统一使用 py -3.12；push 前先确认代理可用，避免 21 秒连接超时。



\## 4. 原理理解

1\. docs/ 保存手写的 Markdown 源文档，site/ 保存构建生成的成品网页。不手工修改 site/ 是因为它每次构建都会被整体覆盖重新生成，手工改动会丢失，且成品应该始终由源码推导出来，保证一致性。

2\. 当 docs/ 目录、mkdocs.yml、requirements.txt 或工作流文件本身被推送到 main 分支时自动运行；也可以在 Actions 页面用 workflow\_dispatch 手动运行。

3\. build 负责取源码、装 Python 和依赖、构建网站并上传 site/ 产物；deploy 负责把产物发布到 Pages。deploy 设置了 needs: build，构建成功才会部署。

4\. 不会。因为工作流 paths 中没有包含 report/，只改报告不触发部署，这是预期行为，可以避免无意义的重复构建。

5\. 本方案使用 GitHub 官方 Pages 工作流，构建产物作为 artifact 直接上传发布，不需要把成品提交到 gh-pages 分支托管，仓库里也就不需要维护这条额外分支。

