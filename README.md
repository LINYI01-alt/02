# Douyin AI Radar / 抖音 AI 智能运营工作台

当前仓库是网站前端，使用 Supabase Auth 与数据库作为后台。网站入口由 GitHub Pages 的 GitHub Actions 工作流发布。

## 当前已接入

- 邮箱注册、登录与 Supabase Row Level Security 数据隔离
- 作品库、关键词、采集任务、原创脚本、审核中心
- AI 工作台：选题研究、原创脚本、作品分析、评论回复草稿
- 抖音 OAuth 网站授权启动与回调服务（等待开发者应用凭证）
- AI 任务、发布队列、评论回复队列的数据表

## 网站发布

1. 在 GitHub 仓库 Settings → Pages 中选择 GitHub Actions。
2. 推送到 main 后由 .github/workflows/static.yml 自动部署。
3. 网站 URL 通常为 https://linyi01-alt.github.io/02/ （以仓库 Pages 设置显示为准）。

## Supabase 项目

项目 ref: tomlqevtbwhoduvuujsu

前端仅使用 Supabase publishable key。绝不可把 service_role、OpenAI API key、抖音 Client Secret、access_token 或 refresh_token 写入 index.html 或其他浏览器可见文件。

## 必要的 Edge Function Secrets

在 Supabase Dashboard → Edge Functions → Secrets 中配置：

- OPENAI_API_KEY：通过 OpenAI Platform 创建的 API key
- OPENAI_MODEL：建议 gpt-6.1-sol（或你有权使用的模型）
- DOUYIN_CLIENT_KEY：抖音开放平台网站应用 Client Key
- DOUYIN_CLIENT_SECRET：抖音开放平台网站应用 Client Secret
- DOUYIN_REDIRECT_URI：https://tomlqevtbwhoduvuujsu.supabase.co/functions/v1/douyin-oauth
- DOUYIN_TOKEN_ENCRYPTION_KEY：32 字节随机密钥的 Base64 字符串（可在终端运行 openssl rand -base64 32 生成；不要提交到 GitHub）
- DOUYIN_APP_URL：GitHub Pages 网站地址（可选，供后续回跳使用）

Supabase Edge Functions 已部署：

- ai-workflow：要求有效 Supabase 用户 JWT
- douyin-oauth：POST 启动时自行验证用户 JWT；GET 仅用于抖音 OAuth 回调

## 抖音开放平台

1. 前往 https://open.douyin.com/ 注册开发者并选择“自研开发者”。
2. 创建并提交网站应用审核，获取 Client Key / Client Secret。
3. 将上述回调地址登记为网站应用的授权回调域名/URL。
4. 按应用实际场景申请所需接口权限与 scope。评论管理需互动管理权限（video.comment）；视频发布与管理按开放平台当前应用权限页面申请。
5. 将 Client Key / Client Secret 设置为 Supabase Edge Function Secrets，再在网站 AI 工作台旁的“抖音授权”页面发起授权。

抖音 OAuth 与接口能力以官方文档及应用审核结果为准。账号授权不代表自动获得所有视频、数据、搜索、发布或评论权限。不得收集抖音密码、抓取私人观看历史或绕过平台访问控制。

## AI 与医学安全

AI 工作流仅生成研究摘要、内容分析和草稿。生育健康、辅助生殖、药物、诊断、治疗和妊娠风险相关内容必须经过可靠来源核验和人工医学审核。评论回复涉及个人诊疗问题时，不自动回复、不收集敏感个人健康信息。

## 当前未完成

- OPENAI_API_KEY 尚待在 Supabase Secrets 中配置（创建 API key 的安全设置流程已启动）
- 抖音开放平台开发者账号、网站应用审核及 Client Key / Secret 尚待用户完成
- 作品/用户数据、发布、互动权限需按应用审核结果接入
- 后台采集 worker、视频渲染与自动发布/评论执行器尚未启用；现有任务队列不会伪装成已采集或已发布
