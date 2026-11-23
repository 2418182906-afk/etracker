# ETF 份额追踪系统：自动更新与网页部署

## 部署结构
- GitHub Actions：每周一至周五北京时间 17:20 自动运行 Python 更新程序。
- GitHub Pages：发布最新的静态网页。
- 网页不需要常驻 Python 服务器；浏览器只读取已经生成的 HTML 和本地 ECharts 文件。

## 部署步骤
1. 在 GitHub 新建一个仓库，例如 `etf-share-tracker`。若使用 GitHub Free，建议仓库设为 Public，GitHub Pages 的免费使用条件以 GitHub 当前计划说明为准。
2. 将本项目文件夹内的全部内容上传到仓库根目录。确保 `update_etf_tracker.py`、`requirements.txt`、`etf_share_tracker.html`、`echarts.min.js`、`data/` 和 `.github/workflows/update-and-deploy.yml` 都在根目录对应位置。
3. 在仓库 Settings → Pages → Build and deployment 中，将 Source 设为 `GitHub Actions`。
4. 确认 Settings → Actions → General → Workflow permissions 允许工作流写入仓库内容（Read and write permissions）。工作流本身也声明了 `contents: write`。
5. 打开 Actions，选择 `Update ETF data and deploy website`，点击 Run workflow，执行第一次更新和部署。
6. 工作流成功后，在 Settings → Pages 或 Actions 部署摘要中查看网页地址。通常形如 `https://你的用户名.github.io/仓库名/`。

## 自动更新时间
工作流设置为周一至周五北京时间 17:20（UTC 09:20）。GitHub 定时工作流可能因平台负载延迟；并非保证精确到分钟。若当日不是交易日，程序通常会保留最近一个可获取的交易日数据。

## 手动立即更新
进入仓库 Actions → Update ETF data and deploy website → Run workflow。成功后网页会重新部署。

## 重要说明
- 本部署方式是静态网页：图表、筛选、阈值设置、CSV 导出等前端功能可用；“每日自动更新”按钮用于提示自动更新机制，不会直接从网页触发服务器同步。需要临时更新时，从 GitHub Actions 手动运行工作流。
- 本项目会访问腾讯财经日线接口和上海证券交易所 ETF 规模接口。若上游接口不可用、限流或数据尚未发布，某次自动任务可能失败或仍显示最近一次成功获取的数据。请在 Actions 中查看运行日志。
- GitHub Pages 是公开网页托管方式。仓库和网页中的数据均不应包含密码、API 密钥、个人隐私或其他敏感内容。
- GitHub 的定时工作流仅在默认分支上触发；请把工作流文件放在默认分支。
