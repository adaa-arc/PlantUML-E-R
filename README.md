# PlantUML 陈氏 ER 图（矩形实体 + 菱形关系且菱形内有文字）教程
 
 > 目标：用 **PlantUML 的 Chen ERD 语法**输出“矩形实体 + 菱形关系”，并且**菱形内部显示关系名**（标准陈氏表示法）。
 > 关键：必须用 `@startchen ... @endchen`（不是 `@startuml`）。
 
 ---
 
 ## 1) 版本要求（必须满足）
 
 - **最低版本**：`plantuml.jar >= 1.2024.5`
   - 原因：`@startchen/@endchen`（Chen ERD）是 1.2024.5+ 才完整支持的语法族。
 - **推荐版本**：直接用最新稳定版（例如 `1.2026.1`）。
 
 ---
 
 ## 2) 插件/预览器如何使用你下载的 `plantuml.jar`
 
 不同 IDE/插件叫法略有不同，但核心是：**让预览器调用你指定的 jar**。
 
 - **VSCode PlantUML 插件**
   - 打开设置，搜索 `plantuml.jar`
   - 设置 `plantuml.jar` 为你下载的 jar 路径
   - 重启 VSCode 后再预览
 
 - **IntelliJ IDEA / Android Studio PlantUML 插件**
   - Settings/Preferences
   - 搜索 `PlantUML` / `Jar`
   - 指定 `plantuml.jar` 路径（或升级插件自带 jar）
   - 重启 IDE
 
 - **验证是否生效**
   - 预览错误窗口通常会显示 PlantUML 版本号（例如 `PlantUML 1.2026.1`）。
   - 如果还看到旧版本（例如 `1.2024.3`），说明插件仍在用旧 jar，需要重新指定/重启。
 
 ---
 
 ## 3) 最小可复制示例（保证“菱形内有文字”）
 
 新建一个 `*.puml`/`*.plantuml` 文件，粘贴后直接预览：
 
 ```plantuml
 @startchen
 entity "用户" as User { }
 entity "商品" as Product { }
 
 relationship "收藏" as FavoriteRel { }
 
 User -N- FavoriteRel
 FavoriteRel -M- Product
 @endchen
 ```
 
 你应该看到：
 - **实体**（`用户`/`商品`）是矩形
 - **关系**（`收藏`）是菱形，且**菱形内有“收藏”文字**
 - 基数 `N/M` 显示在连线上
 
 ---
 
 ## 4) Aliases（别名）正确写法（最容易写错）
 
 在 `@startchen` 里，带别名的标准写法是：
 
 - **实体**：
 
 ```plantuml
 entity "显示名" as ALIAS { }
 ```
 
 - **关系**：
 
 ```plantuml
 relationship "显示名" as ALIAS { }
 ```
 
 ### 常见错误（会直接 Syntax Error）
 
 - **错误**：`entity ALIAS as "显示名" { }`
 - **正确**：`entity "显示名" as ALIAS { }`
 
 ---
 
 ## 5) 基数标注写法（1/N/M）
 
 Chen ERD 用连接符表达基数，常见有：
 
 - `-1-`
 - `-N-`
 - `-M-`
 
 示例：
 
 ```plantuml
 @startchen
 entity "用户" as User { }
 entity "地址" as Address { }
 relationship "拥有" as Own { }
 
 User -1- Own
 Own -N- Address
 @endchen
 ```
 
 ---
 
 ## 6) 布局技巧（让全局 ER 图不乱）
 
 - **横向布局**：
 
 ```plantuml
 @startchen
 left to right direction
 ' ...
 @endchen
 ```
 
 - **减少交叉建议**
   - 先定义“中心实体”（例如用户/商品）
   - 按模块分组定义关系（用户相关、商品相关、订单相关、聊天相关）
 
 ---
 
 ## 7) 全局 ER 图模板（可直接套用）
 
 ```plantuml
 @startchen "数据库全局E-R图"
 left to right direction
 
 entity "用户" as User { }
 entity "商品" as Product { }
 entity "订单" as TradeOrder { }
 
 relationship "发布" as Post { }
 relationship "提交" as Submit { }
 relationship "包含" as Include { }
 
 User -1- Post
 Post -N- Product
 
 User -1- Submit
 Submit -N- TradeOrder
 
 TradeOrder -N- Include
 Include -1- Product
 
 @endchen
 ```
 
 ---
 
 ## 8) 常见报错与排查
 
 - **No valid @start/@end found**
   - **原因**：当前渲染器不认识 `@startchen`（版本太低或插件没用到你新 jar）
   - **解决**：
     - 确认预览窗口显示 PlantUML 版本 >= `1.2024.5`
     - 确认插件配置的 jar 路径正确
     - 重启 IDE
 
 - **Syntax Error? (Assumed diagram type: chen_eer)**
   - **原因**：大概率是别名写反：`entity ALIAS as "显示名"`
   - **解决**：改为 `entity "显示名" as ALIAS`
 
 - **菱形存在但没有文字**
   - **原因**：你没用 `@startchen`，而用 `@startuml` 的“模拟画法”（例如 `choice`）
   - **解决**：切换到 `@startchen`，用 `relationship "xxx"` 定义关系
