\# 使用指南



1\. 修改 docs 目录中的 Markdown 文件。

2\. 在本地预览并检查页面及链接。

3\. 提交修改并推送到 main 分支。

4\. 在 Actions 页面查看构建与部署状态。

5\. 打开网站检查内容是否更新。



\## 常用命令



\- python -m mkdocs serve：本地预览。

\- python -m mkdocs build --strict：构建网站并严格检查警告。



\## 提交前自查



| 检查项 | 命令 |

| --- | --- |

| 查看改动了哪些文件 | git status |

| 确认暂存内容正确 | git diff --staged |

| 查看最近提交记录 | git log --oneline -n 3 |



\## 我的环境备注



\- 系统：Windows 11，终端使用 Git Bash。

\- Python：统一用 py -3.12

\- 文件编码：全部保存为 UTF-8，防止中文乱码。



接口文档写法见 \[API 参考](api.md)。

