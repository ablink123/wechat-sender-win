# product-page 目录约定

## 用途

用于存放 Windows11 个人微信电脑端发送 SKILL 的静态产品介绍页。

## 目录结构

- `index.html`：产品落地页入口，内联本页所需 CSS 和少量交互脚本。
- `developer-api.html`：开发接口独立说明页，面向不依赖桌面 Agent、直接调用本地 CLI 的第三方系统开发者。
- `rc4-preview.html`：仅供本地临时评审，不纳入 Git 或正式发布目录；正式内容确认后合并到 `index.html`。
- `buy-test.html`：微信支付小额验收页；仅用于受控测试，不从公开购买按钮进入。
- `buy.html`：正式购买页保留名称；只有真实支付验收完成后才从 `buy-test.html` 整理生成。
- `assets/`：页面直接引用的图片、视频和说明资料副本。
- `assets/previews/`：本地页面评审截图目录，不纳入 Git 或正式发布目录。
- `assets/downloads/`：产品页提供的测试发布包；文件名必须包含版本号，页面同步标注 SHA256 和用途。
- `assets/vendor/`：静态页必须在浏览器运行的第三方前端库；保留对应许可证说明，不放服务端 SDK。

## 命名约定

- 页面文件使用小写 kebab-case。
- 素材文件保持来源文件名，避免和上游 `upload/` 目录脱节。

## 修改规则

- 不删除 `assets/` 中的来源素材副本，替换素材时覆盖同名文件并记录来源。
- 下载包不嵌入授权码、密钥或本地授权状态；授权码必须通过授权中心单独发放。
- RC3 首次联网自动开启 7 天体验，产品页不得再提供人工体验码申请入口；付费授权码仍由购买流程单独发放。
- 产品页必须明确客户端双击后主要是试用/授权状态界面；自然语言发送还需要导入 Agent Skill。
- 购买链接、价格、联系方式未提供时不虚构，只保留可替换占位逻辑。
- 支付页只能调用授权中心公开 API；微信支付密钥、商户私钥、Supabase service role key 和测试购买令牌不得写入静态文件。
- 测试购买令牌只允许测试者在页面运行时输入，并仅保存在当前标签页的 `sessionStorage`。
- 页面必须能在本地直接打开；如引入构建工具，需先更新本文件说明。
- 新版大改先在独立预览页完成桌面端和移动端验收；用户确认后再把已验证内容合并到 `index.html`。
- RC4 预览页只引用 `assets/downloads/微信本地通知助手-v2.0.1-rc4-setup.exe` 这一份安装器；不得恢复客户端与 Skill 分开下载的旧流程。
- 产品页需要区分“用户安装方式”和“网站发布方式”：用户只下载安装器；网站发布采用 GitHub `main` 提交后由 Netlify 自动部署，不使用 Netlify CLI 直接覆盖生产站点。
- V2.1.0 RC1 首页更新范围限定为 `index.html` 的 `#download` 内容及站内下载 CTA 的目标链接；不得重做首页其他区块的布局和视觉风格。
- `#download` 必须延续现有双栏卡片和深蓝/红/白视觉体系，只提供一个 V2.1.0 RC1 安装器下载；Skill ZIP 与 Developer Kit 已随安装器安装，不再作为网站第二下载项。
- `#download` 区域只在左侧主卡片保留一个“主下载”按钮；右侧 Skill 说明卡不得重复提供安装器下载按钮。
- V2.1.0 RC2 更新只允许新增已验收的 RC2 安装器，并同步修改 `index.html` 中现有三个安装器 CTA、版本、文件大小、SHA256 和未签名提示；不得删除旧下载资产、改动购买流程或重做页面视觉。
- RC2 网站仍只公开一个 Windows 安装器；release ZIP、独立 Skill ZIP 和 Developer Kit 不增加为公开下载按钮。
- V2.1.0 RC2 开发接口页更新范围限定为 `developer-api.html` 的三个下载 CTA 直接指向 RC2 安装器，并同步页面版本展示和 Result V1 示例中的 `app_version`；不改 Payload/Result 协议、发送边界、页面结构或视觉。
- 开发接口页必须以已安装的 `%LOCALAPPDATA%\WechatSender\wechat-sender.exe` 为稳定边界；V2.1.0 RC2 继续对外公开 Payload V1、`--validate-payload`、`--output-json` 和 Result V1，标准输出只用于诊断。默认示例保持 `send=false`，不得公开源码、授权密钥、授权中心数据库或服务端内部接口。
- 开发接口页可提供 `--contacts-file` 作为普通用户手工批量调用方式，但必须说明：程序化接入、附件、重试控制和任务记录仍推荐 `--payload-json`；直接位置参数不带 `--send` 会写入微信草稿，不等同于 Payload 或联系人文件的纯预览。
