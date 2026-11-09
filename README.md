# AI-Computer-Free
给到Astrbot使用的插件，安装后输入boxai的链接link即可使用，默认管理员可用，其他人使用可添加QQ等
Simular box 云电脑申请地址：https://box.simular.ai/

可上传安装的 AstrBot 插件。把 Simular 的完整共享链接填入插件配置后，AstrBot 就能使用共享云电脑的命令行、浏览器、桌面及文件工具。

适用 AstrBot 4.13.x 及更新的 4.x 版本，Python 3.12+。机器人所用模型需要支持工具调用；看图和按截图操作还需要视觉能力。

安装

在 AstrBot 管理面板的插件页面选择上传插件，上传 astrbot_plugin_simular_box-1.0.0.zip。
打开该插件配置，将完整的 https://box.simular.ai/s#box=…&key=… 链接粘贴到 shared_link，保存。链接不要发到机器人聊天或公开群里。
将你的账号加入 AstrBot 管理员。插件默认只允许管理员使用；额外用户可以填入 allowed_users，格式为 平台实例ID:用户ID。
向机器人发送 /box 状态。看到连接正常后即可开始使用。
在 AstrBot 工具页面启用 box_tools、box_call、box_shell、box_screenshot。如果人格配置有工具白名单，也需加入这四个工具。模型工具执行超时建议设为至少 75 秒，需大于插件的 call_timeout。
插件包不包含任何真实共享链接或访问密钥。配置含密钥，仅在管理面板保存；如果备份或分享 AstrBot 配置，请先清除 shared_link。

使用

用 box 打开 Chrome，搜索 Simular，并总结搜索结果。
用 box 运行 Python，生成一个 CSV 文件并告诉我保存位置。
查看 box 当前桌面，告诉我页面上有什么。
/box 状态：检查连接并列出工具数量。
/box 工具：列出云电脑提供的工具。
/box 截图：把当前桌面截图发送到聊天。
/box 身份：查看当前平台实例和账号的授权标识。
box_shell 直接执行 Bash；box_tools(name="browser_navigate") 获取原始参数格式，随后 box_call 调用浏览器、桌面或文件工具。box_screenshot 和含截图的 box_call 返回真正的图像内容，供支持视觉的模型使用，不会把图片转成一长串文字。/box 截图 则直接发送图片给用户。

连接和权限

共享链接和个人账户 MCP 登录使用不同入口。插件自动从共享链接提取机器 ID 和密钥，连接 https://mcp.box.simular.ai/mcp/<机器ID>，无需 OAuth 登录，也无需手工添加 AstrBot MCP 服务。

命令与每次模型工具调用都会检查使用者权限。额外用户使用平台实例 ID 和用户 ID 的组合，避免不同机器人连接上同名账号共用权限。共享链接本身也需要允许 AI 操作；只读链接不能执行命令或点击。

同一个插件实例串行执行工具调用，因为 Bash 和桌面状态由使用者共用。初始工作目录是云电脑上的 /workspace。本机文件不会自动上传到 box；生成的文件也不会自动下载到 QQ 或本机。

超时或连接中断时，插件不会重放原操作，避免重复提交或重复写入。应先查看页面、文件或任务状态，再决定下一步。修改共享链接后下一次调用会使用新连接；更改超时设置后请重载插件。

故障排查

连接正常但工具不能执行：检查共享方是否打开“允许 AI 操作”，以及链接是否已被撤销或过期。
只有普通回答，没有调用工具：检查模型工具能力、工具启用状态和人格工具白名单。
能执行命令但看不了图：使用支持视觉的模型；/box 截图 仍可直接发送图片给用户。
工具执行超时：把 AstrBot 模型工具执行超时设为大于插件 call_timeout。长任务应后台执行并保存结果，然后查询进度。
Docker 中无法连接：检查 AstrBot 容器能否访问 mcp.box.simular.ai:443。插件在 AstrBot 所在环境直接发起 HTTPS 请求。
截图缓存存放在 AstrBot 的 data/plugin_data/astrbot_plugin_simular_box/screenshots/，保留最近 64 张。

来源：Simular box、AstrBot 插件文档。
