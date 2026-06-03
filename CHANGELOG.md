# Changelog

ChronicleX 所有可见变更记录在此文件。

格式参考 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/),版本号遵循 [Semantic Versioning](https://semver.org/lang/zh-CN/)。

---

## [1.3.0] - 2026-06-03

### Added

- **农历重复事项**:事项可按**农历每年**重复(生日 / 忌日 / 纪念日),每年自动落在正确的公历日。重复编辑器在「每年」频率下多一个「农历」勾选 + 只读确认标签「农历八月十五」,**锚点农历月日由公历开始日自动换算**(无新增选择器)。农历固有边界已定策略:**闰月只在正月份触发一次**(正月号天然不命中闰月)、**月小无三十落该月最后一天**(廿九,不丢年)。复用现有 `interval`(每 N 农历年)/ `count` / `until`(公历结束日)与改/删范围(此条 / 未来 / 全部);提醒按**下一个公历实例**触发(horizon 加宽到 400 天覆盖农历年最大间隔)。旧公历重复事项零影响(`calendar` 缺省即公历,旧存档零迁移)。

### 其它

- 分层:`RecurrenceRule` 加可选 `calendar?: 'solar'|'lunar'` / `lunarMonth?` / `lunarDay?`;纯函数 `domain/lunar.ts#getLunarYmd`(公历→农历数字年月日)+ `domain/recurrence.ts#generateLunarYearlyOccurrences`(逐农历年反查公历,`lunar-javascript` 的 `Lunar.fromYmd`/`LunarMonth.fromYm`);`expandEventsInRange` 与 `reminder.ts#nextOccurrenceDate` 加农历分支(**公历路径零改动**)。subagent-driven 执行(6 task,domain 全 TDD;final review 准予合并)。测试 354 → **372**(getLunarYmd ×3 / 农历生成器 ×10 含闰六月2025真实闰月单触发 / expand 分支 ×1 / 提醒分支 ×1 / RecurrenceEditor ×3)。spec+plan 见 `docs/superpowers/{specs,plans}/2026-06-03-lunar-recurrence*`。**已知限制**:被单独编辑过的重复实例(`isRecurrenceException`)不单独触发提醒,仍按母序列下一个实例算(与既有重复一致)。

---

## [1.2.0] - 2026-06-03

### Added

- **多窗口同步(切窗即刷新)**:同设备多个思源窗口(拖出的独立窗口 / 第二个窗口)各自加载插件、各有独立事项 store,读写同一内核文件。窗口重新获得**焦点 / 变为可见**时从文件重载事项,使该窗所有视图(日历 / Dock / 日 / 周)自动刷新到最新。新增纯函数 `domain/sync.ts`(`eventsChanged`,按 id + updatedAt 比对)+ `store/events.ts#reloadEventsIfChanged`(本窗有未落盘写时跳过,防覆盖本地编辑)。仅事项;无 WebSocket / 轮询 / 新依赖。测试 343 → 354。

### Fixed

- **写状态守卫**:`hasPendingWrite` 原依赖 `pendingPromise`,写完不置 null 致任一编辑后永久非空、聚焦重载被永久跳过;改用 `dirty` 标志(写**成功**才清,失败保留以防 reload 用旧文件覆盖未落盘数据)。

---

## [1.1.0] - 2026-06-03

### Added

- **农历显示**:月 / 周 / 日视图显示**农历日 + 二十四节气 + 传统节日**。优先级 传统节日 > 节气 > 农历日,每天取最高一项显示;节气 / 传统节日用**主色 + 加粗**双重强调(色盲友好,非只靠色)。月 / 周视图在公历数字旁显示农历小字(DayCell 复用,一处覆盖两视图),日视图头部显示完整农历(农历月日 + 节气 / 节日 chip)。设置面板加「显示农历」开关(默认开,即时生效)。数据源 [lunar-javascript](https://github.com/6tail/lunar-javascript);配色全程 `--chx-*` token 跟随明暗主题。新增纯函数 `domain/lunar.ts`(`getLunarInfo`)+ `Config.showLunar`。

---

## [1.0.1] - 2026-06-03

### Changed

- **节假日配色重设计(暖金橙层次化)**:放假日由「思源报错红」改为「warning 暖金」—— 底色极淡(`warning 7%`)、「休」badge 暖金强调、**节日名改中性次要色**,终结红色三层堆叠;调休上班日维持中性灰,放假 / 调休靠 暖金 vs 灰 + 休/班 文字双重区分(色盲友好)。月视图 DayCell / Dock 迷你日历 / 顶部节假日栏三处统一。经 ui-ux-pro-max 设计,全程 `--b3-*` token 跟随明暗主题。

### Added

- **plugin.json `funding` 赞赏字段**:思源集市插件详情页渲染赞赏入口(指向赞赏码)。

### Docs

- README 双语:**问题反馈移到开头**(简介后、功能前)便于他人反馈;**版本历史精简**(仅保留近期主版本 v1.0.0~v0.8.0,更早折叠引导 CHANGELOG);移除「自定义状态名」候选功能。

---

## [1.0.0] - 2026-06-02

> 🎉 ChronicleX 首个正式版 1.0.0 —— 经一轮全页面 UI 重设计定稿。

### Changed

- **全页面 UI 重设计(Soft UI + Bento 混合形态)**:在现有 iOS-style token 体系上换**形态语言**,统一全 31 个 Svelte 视图的视觉与交互。主日历 / 周 / 日 / 编辑器 / 对话框套 **Soft UI**(多层柔和阴影 + 稳定 hover);复盘看板与 Dock 套 **Bento**(模块卡片 + 不等格栅 —— 复盘宽面板下「分布 / 拖延」并排、「概览 / 热力图」满宽,Dock 窄面板单列卡片)。配色仍全程 alias 思源 `var(--b3-*)`,自动跟随明暗主题(换形态不换色;glassmorphism 因思源面板无 vibrant 背景已排除)。

### Fixed

- **暗色主题硬编码色**:EventCard 节假日警告文字 `#856404` 无 fallback、OverviewCard 待办色误用未定义变量 `--chx-warning-color`(永远 fallback 到硬色 `#ff9500`)→ 暗色下不可读 / 不跟随主题。改为 `var(--b3-card-warning-color, …)` / `var(--chx-warning)`。
- **键盘焦点环作用域遗漏**:DockView 用历史前缀 `chroniclex-`(非 `chx-`),被全局 focus 环 scope 漏掉 → 补 `chx-dock` class 纳入。
- **导航按钮无障碍标签不准**:CalendarView 上 / 下按钮 `aria-label` / `title` 硬编码「上一月 / 下一月」,但周 / 日视图实际按周 / 天翻 → 改随视图动态(月「上一月」/ 周「上一周」/ 日「前一天」)。

### 其它

- **设计 token 增量**(`ios-tokens.scss`):字号刻度补档 `--chx-fs-{micro,caption,callout,headline,title-lg,stat,stat-sub,display}`(贴合现有散值,零视觉跳变);Soft UI 多层阴影 `--chx-shadow-{sm,md,lg}`(明暗双套,暗色叠 1px 内描边补层次);动效 `--chx-dur-{fast,base,slow}` + `--chx-ease`;Bento 几何 `--chx-bento-{gap,radius,pad}`;`--chx-focus-ring`。
- **全局基础设施**(`index.scss`):`:focus-visible` 焦点环 + `prefers-reduced-motion` 兜底,均 scope 到 `[class^="chx-"]` 容器,**不污染思源宿主 UI**。
- **一致性**:消除 60+ 处裸 `font-size: NNpx`,全部 → 字号 token(grep 断言 0 裸值);8 个图标按钮补中文 `aria-label`;对话框 / 选择器过渡时长统一到动效 token;hover 微交互一律 `translateY` + 阴影升级,不用 `scale`(思源窄面板会抖)。
- **已知限制 / 推迟 v2**:Dock 标题 `📅` emoji 未替换为 SVG(思源 symbol 渲染需运行时验证,无法在构建期自测,暂留)。
- 测试 **337** 全绿(纯样式重构,无逻辑回归);`pnpm run build` 产出 `package.zip`。**执行中额外发现并修复**:focus scope 漏 DockView、导航 label 视图不准。subagent-driven 执行(10 task / 5 阶段,每任务过 spec 合规独立评审)。spec + plan 见 `docs/superpowers/{specs,plans}/2026-06-02-ui-redesign-soft-bento*`。

---

## [0.11.0] - 2026-06-02

### Added

- **提醒 / 通知**:事项到点弹**系统桌面通知**(`Notification` API;未授权/不可用回退思源 toast)。每事项在编辑器「提醒」段单独设提前量——带时间事项:不提醒 / 准时 / 提前 5·15·30·60 分钟·1 天;全天事项:不提醒 / 当天 / 提前 1·2 天(锚点为全局「全天事项提醒时刻」,默认 09:00)。设置面板新增「提醒」段:全局开关 + 全天提醒时刻。点击通知聚焦思源 + 打开该事项编辑器。**思源关闭期间错过的提醒**在下次启动时合并成一条「N 个提醒错过」补提(点击跳日历)。已完成/已取消事项不提醒;重复事项只提醒「下一个实例」。

### Fixed

- **TimePicker 当前选中项 hover 时白字看不清**:CSS 特异性 `.item:hover`(0,2,0)覆盖 `.item--active`(0,1,0)的背景但保留其白色文字 → 选中的时/分被 hover 时白字落到浅色 hover 底上不可读。补 `.item--active:hover` 保持主色底。

### 其它

- 分层:纯函数 `domain/reminder.ts`(预设表 / `computeAnchor` / `nextOccurrenceDate` 委托 recurrence / `computeFireTime` / `selectDueReminders` 到点+未提醒+未完成+全局开筛选 / `reminderKey` 去重 / `pruneNotified` 30 天剪枝 / `planNotifications` 启动汇总 vs 逐条);`store/reminders.ts`(60s 轮询调度器 + 可测的 `runReminderTick` 注入式编排 + 状态持久化 `chroniclex-reminders.json`);`siyuan/notify.ts`(系统通知 + toast 回退 + 权限请求);`index.ts` onLayoutReady 启调度、onunload 停。`Config` 加 `reminderEnabled`/`allDayReminderTime`(`normalizeConfig` 白名单校验,空存档走完整归一化补全默认)。**已知限制**:被单独编辑过的重复实例(`isRecurrenceException`)不单独触发提醒,仍按母序列下一个实例算。测试 294→**320**(reminder ×20 / config reminder ×3 / reminders store ×3)。subagent-driven 执行(8 task,纯函数走 TDD;final review 修 config 空存档默认补全 + 例外注释 + 已知限制)。spec+plan 见 `docs/superpowers/{specs,plans}/2026-06-02-reminders*`。

---

## [0.10.0] - 2026-06-02

### Added

- **事项颜色自定义**:设置面板新增「颜色」段(位于「常规」与「节假日」之间),三行(高优先级 / 普通 / 低优先级)各一排 8 色预设色块(红/橙/黄/绿/蓝/紫/粉/灰)。点击色块即时生效,无需确认或重载。EventCard 优先级条、EventBar 优先级条+背景 tint、EventEditor 优先级 SegmentedControl 全部跟随;月/周/日视图通用,移动端自动跟随。仅 priority 级别自定义,不改 Event 数据模型(无 per-event 独立选色)。

### 其它

- 实现:纯函数 `domain/color.ts`(`DEFAULT_PRIORITY_COLORS`/`PALETTE` 8 色/`normalizePriorityColors` 校验 fallback/`resolvePriorityColor`);`Config` 加可选 `priorityColors`,`normalizeConfig` 白名单校验(非法 hex 逐键 fallback 默认);`index.ts` onload 注入 CSS 变量 `--chx-priority-{high,normal,low}` 到 `document.documentElement` + `configStore` 订阅实时更新(`onunload` 退订防泄漏);EventCard / EventBar 改用 `var(--chx-priority-*, var(--原默认 token>))` fallback 链。
- **修复(开发期反馈)**:EventEditor 优先级 SegmentedControl 在「已打开的弹窗」里改色不实时更新(关闭重开才生效)。根因:`PRIORITY_COLORS` 是 `const` + CSS var 字符串,inline `style` 在 `:root` 变量改时 Chromium/Electron 不可靠重绘。改为 `$: PRIORITY_COLORS = $configStore.priorityColors ?? DEFAULT`(喂实际 hex):配置变 → inline 字符串真变 → Svelte 重渲染 → 重绘;并彻底摆脱跨 document 的 `:root` 依赖。
- 测试 276→**294**(color ×11 / config priorityColors ×4 / EventEditor 优先级色随 config ×2 + 现有)。subagent-driven 执行(8 task,纯函数走 TDD)+ 实时更新修复(systematic-debugging,失败测试先行)。spec+plan 见 `docs/superpowers/{specs,plans}/2026-06-01-priority-color-customization*`。

---

## [0.9.0] - 2026-06-01

### Added

- **导入导出(JSON 备份/迁移)**:设置面板新增「数据」组。**导出**全部事项(原始未展开 recurrence、完整保真)为 `chroniclex-backup-YYYYMMDD-HHmmss.json`。**导入** JSON 文件:校验 schema + 逐项校验(非法跳过计数),选「合并」(按 id 去重,只补本地缺失)或「替换」(二次确认后清空覆盖,文案含现有条数);文件无有效事项时拦截不弹窗(防误清空)。完成报告新增/跳过/忽略数。纯函数 `domain/io.ts`(serialize/parse/merge/filename)收口、`libs/file-io.ts` 浏览器 IO、`store/events.ts#replaceAllEvents`。仅事件数据(不含 config);桌面优先,移动端受 webview 限制不保证。

### 其它

- 测试 263→**276**(`domain/io.ts`:serialize/parse 合法非法缺字段版本/null/merge 去重计数/filename;`replaceAllEvents`)。spec+plan 见 `docs/superpowers/{specs,plans}/2026-06-01-import-export*`。

---

## [0.8.1] - 2026-06-01

### Added

- **已绑定文档「打开」按钮**:编辑事项弹窗的「已绑定」行在「解除」旁新增「打开」,点击经 `openDoc(linkedDocId)` 在思源主区打开关联文档 tab(不关编辑弹窗,避免丢未保存改动;文档已删则 `openDoc` 内置 `showMessage` 提示)。

---

## [0.8.0] - 2026-06-01

### Added

- **周/日视图切换**:日历工具栏新增「月 / 周 / 日」三段切换,选择持久化到 `config.view`(下次打开 tab 恢复)。周视图 7 列(复用 `DayCell`,列头星期+日期、今天高亮、节假日角标、跨列拖拽改期、列内滚动显示当天全部);日视图单日列表(复用 `EventCard`,状态切换/删除/编辑、节假日头、空态)。导航统一为锚定日期 `anchorDate`,prev/next 按当前视图步进(月±1月/周±7天/日±1天),标题随视图变。

### Changed

- 周/日视图经纯函数 `expandEventsInRange` **展开重复事项**(各实例可见);月视图维持原行为(不展开,见 spec 已知不一致)。
- 文档反查「跳日历」改为落**日视图**定位当天(原为月视图弹当日详情)。

### 其它

- 新增 `domain/date.ts#addMonths`/`getWeekRange`、`domain/recurrence.ts#expandEventsInRange`(`store/events.ts` 委托)、`components/WeekView.svelte`、`components/DayView.svelte`;`DayCell` 加 `maxBars` prop;`config` 加 `view` 字段。测试 256→**263**(addMonths/getWeekRange/expandEventsInRange/config.view 用例)。subagent-driven 执行(7 task,纯函数走 TDD;final review 修 2 处节假日反应式缺口:DayView chip 改 `$holidaysStore` 派生、周视图跨年周补加载)。spec+plan 见 `docs/superpowers/{specs,plans}/2026-06-01-week-day-view*`。

---

## [0.7.2] - 2026-06-01

### Fixed

- **文档反查菜单项不出现**:v0.7.0 接线在 `click-editortitleicon`(点文档标题图标),但新版思源点标题图标弹出的是 emoji/图标选择器、不再弹菜单,故「ChronicleX 关联事项」项始终不显示。改接 `open-menu-doctree`(文档树右键菜单),从右击节点 `data-node-id` 取 docId,仅 `type==='doc'`(单文档)时注入。现于**文档树右键菜单**可见。

---

## [0.7.1] - 2026-06-01

### Fixed

- **长标题事项条撑破日历格子**:月视图某天事项标题过长时,事项条横向溢出、把所在列(grid 1fr)撑宽,导致整月布局错乱。根因是 grid/flex 项默认 `min-width:auto`(=内容宽)不收缩。修法:`DayCell` grid 项 `.chx-day` + `.chx-day__bars`、`EventBar` `.chx-bar` + `.chx-bar__title` 链路统一加 `min-width:0`,长标题正确截断为 `…`,列宽恒定。

---

## [0.7.0] - 2026-06-01

### Added

- **文档反查关联事项**:在思源**文档树右键菜单**新增「ChronicleX 关联事项 (N)」项(N=关联数)(注:v0.7.0 误接文档标题图标菜单不生效,v0.7.2 修正),点击列出 `linkedDocId` 指向本文档的事项;每行「编辑」(开编辑器)/「跳日历」(打开月视图定位到事项日期 + 当日详情)。补全正向(事项→文档)的反向链路。

### 其它

- 新增纯函数 `domain/search.ts#findEventsByDoc`、`store/focus.ts`(跳日历信号 `focusDateStore`)、`siyuan/tab.ts`(`openCalendarTab`)、`libs/event-launch.ts`(抽取 EventEditor 启动收口)、`siyuan/docMenu.ts`(eventBus `click-editortitleicon` 接线)、`components/LinkedEventsDialog.svelte`。CalendarView 改用共享 launcher(行为等价)+ 订阅 `focusDateStore` 导航。测试 240 → **246**(findEventsByDoc ×4 / focusDateStore ×2)。spec+plan 见 `docs/superpowers/{specs,plans}/2026-06-01-doc-backlink-events*`。

---

## [0.6.6] - 2026-06-01

### Changed

- **窄屏布局优化(第二批)**:移动端三处组件内部窄屏排布优化——① 月视图格子紧凑化(`DayCell`/`MonthGrid`/`EventBar` 缩 padding/gap、`min-height` 90→60px 减滚动、事项条更可读、节日名隐藏让位给「休/班」角标);② 设置面板行纵向堆叠(`defaultNotebookId`/`defaultDocPath`/`weekStart` 控件占满宽好填);③ EventEditor 时间段在窄屏整体掉到第二行满宽、不再换行错位。

### 其它

- **桌面零副作用**:A/C 全用 `@media (pointer: coarse)`(桌面 `pointer:fine` 不命中)+ EventEditor 时间段 wrapper 桌面 `display:contents` 透明;B 新增纯函数 `libs/settings-layout.ts#settingsRowStyle`,桌面分支与原内联样式等价。移动深度化子项目 2(窄屏布局)第二批。测试 237 → **240**(settingsRowStyle ×3)。spec+plan 见 `docs/superpowers/{specs,plans}/2026-06-01-narrow-layout-batch2*`。

---

## [0.6.5] - 2026-05-31

### Changed

- **窄屏弹窗适配**:移动端经 `svelteDialog`/`simpleDialog` 挂载的弹窗(EventEditor/DocPicker/DayDetailDialog/ScopeSelector)容器宽度统一 `92vw`,不再溢出窄屏。新增纯函数 `libs/dialog-width.ts#resolveDialogWidth` 在 `simpleDialog` 一处收口(呼应已有 max-height 约定)。
- DayDetailDialog/DocPicker/ScopeSelector 根 `min-width` 改 `min(Xpx, 100%)`,窄屏不再撑破容器。

### 其它

- **桌面零副作用**:`resolveDialogWidth` 桌面分支返回原宽度;`min(Xpx,100%)` 在宽容器恒取 Xpx,与原 `min-width:Xpx` 等价。移动深度化子项目 2(窄屏布局)首项。测试 233 → **237**(resolveDialogWidth ×4)。spec+plan 见 `docs/superpowers/{specs,plans}/2026-05-31-narrow-dialog*`。

---

## [0.6.4] - 2026-05-31

### Added

- **移动端拖拽排序**:移动端 DayDetailDialog(点某天弹出的详情)新增「排序模式」——点标题栏「↕」进入,每张普通事项卡左侧出现 ≡ 手柄,按住拖动调整同一天事项顺序,点「完成」退出并持久化。新增 `domain/reorder.ts`(纯函数:reorderArray/assignOrders/computeInsertIndex)+ `actions/dragSort.ts`(仅 touch 手柄拖拽 action)。

### Changed

- `Event` 加可选 `order` 字段;`sortEvents` 同一天内加 order 档(有 order 沉顶、按序排;无 order 走原五级规则)。某天被手动排过后整列按手动顺序,**未拖过的天行为不变**。

### 其它

- **桌面零副作用**:排序入口/手柄仅 `getIsMobile()` 为真时挂载;`dragSort` 仅 `pointerType==='touch'` 介入。重复事项不可拖、沉底;排序模式下全部卡(含重复)用透明 shield 隔离 tap/长按/左滑(EventCard/swipe/longpress 零改动)。测试 214 → **233**(reorder ×8 / search ×3 / events ×2 / DayDetailDialog ×6)。spec+plan 见 `docs/superpowers/{specs,plans}/2026-05-31-mobile-drag-sort*`。

---

## [0.6.3] - 2026-05-31

### Added

- **移动端底部操作栏 + 悬浮 FAB**:CalendarView 在移动端底部新增固定操作栏(今天 / 搜索 / 筛选)与右下悬浮「＋」新建按钮。新增受控组件 `MobileActionBar.svelte`(纯回调底栏 + FAB,无 state)。
- **底部弹出筛选面板(bottom sheet)**:把原顶部的搜索框 / 状态 chip / 标签 chip 下放到从底部升起的 `FilterSheet.svelte`(受控:props 传值 + `onSearch/onToggleStatus/onToggleTag` 回调上报,单向数据流)。

### Changed

- 移动端隐藏 CalendarView 顶部 `.chx-calendar__filters` 段(筛选改由底部面板承载),为窄屏腾出空间。

### 其它

- **桌面零副作用**:移动组件用 `{#if getIsMobile()}` 条件渲染(桌面恒为 false,DOM 根本不挂载)+ 组件内 `@media` 双保险;CalendarView 持有唯一一份 filter state,桌面顶部控件与移动 sheet 共用。测试 200 → **214**(MobileActionBar ×5 / FilterSheet ×9)。spec+plan 见 `docs/superpowers/{specs,plans}/2026-05-31-mobile-bottom-bar*`。

---

## [0.6.2] - 2026-05-30

### Added

- **下一个节假日倒计时**:月视图顶部「本月节假日」栏右侧显示「距 {节日名} 还有 N 天」;当天正逢放假日时显示「{节日名} · 假期中」。新增纯函数 `domain/holiday.ts#findNextOffDay`(最近放假日)+ `domain/date.ts#daysBetween`(自然日差)。

### Changed

- 顶部「本月节假日」栏显示条件放宽:本月无节假日但有下一个假期时,仍显示该栏(只含倒计时)。

### 其它

- 纯展示、桌面 / 移动通用;只在当年已加载数据里找下一个放假日,跨年 / 无数据时隐藏(不主动加载次年数据)。测试 189 → **200**。spec+plan 见 `docs/superpowers/{specs,plans}/2026-05-30-next-holiday-countdown*`。

---

## [0.6.1] - 2026-05-30

移动深度化**子项目 3(移动专属交互)首项**:事项卡 iOS 风「左滑露出删除」。

### Added

- **swipe-to-delete**:移动端事项卡(EventCard)左滑露出红色「删除」区,点它删除,滑回 / 点别处收回(iOS 标准、天然防误删);露出过半吸附展开、不足回弹。新增 `domain/swipe.ts`(方向判定 / 露出 clamp / 吸附,纯函数)+ `actions/swipe.ts`(仅 `pointerType==='touch'` 的 Pointer Events action)。

### Changed

- **移动端隐掉常显 `−` 删除按钮**:由 swipe 取代(桌面 hover `−` 保留不变)。

### 其它

- 仅 touch 介入,桌面零副作用(鼠标 / 点击编辑 / 状态切换 / 拖拽改期不变);方向判定避免与列表纵向滚动冲突,swipe 后吞 click 防误进编辑。测试 177 → **189**(swipe 纯判定 12)。spec+plan 见 `docs/superpowers/{specs,plans}/2026-05-30-swipe-to-delete*`。

---

## [0.6.0] - 2026-05-30

思源移动端**首次开启**:`frontends` 增加 `mobile` / `browser-mobile`,ChronicleX 正式在思源手机端可用。经 browser-mobile(手机浏览器连桌面思源)真机走查通过,核心链路可达可用、无功能性阻断,桌面端零回归。

> 这是「深度移动化」三子项目的第一步(**开启 + 基础可用**);窄屏布局优化、移动专属交互(swipe 等)留后续版本。

### Added

- **移动端 frontend 开启**:`plugin.json` frontends 增加 `mobile` + `browser-mobile`,插件正式在思源手机 App 与移动浏览器可用。
- **真机走查通过**:看月历 / 今日清单、新建·编辑·完成事项、关联思源文档、节假日显示在移动端可达可用;长按菜单(500ms)、拖拽改期(400ms)触摸有效——v0.5.x 预埋的 longpress / draggable / openMenuForEvent 首次真机验证。

### Changed

- **设置 Dialog 移动端宽度自适应**:窄屏用 `92vw` 防溢出(桌面仍 `540px`,零副作用)。

### 其它

- 复用 v0.5.x 已埋的 isMobile / longpress / draggable / openMenuForEvent,本版为「开启 + 真机验证 + 边界修复」;测试维持 **177** 全绿,`pnpm run build` 通过。mobile App(原生)补充验证留发版后。spec+plan 见 `docs/superpowers/{specs,plans}/2026-05-30-v0.6-mobile-enable*`。

---

## [0.5.2] - 2026-05-30

为桌面专属的「右键菜单」交互补一条移动端「长按」触发路径,使后续开启思源移动端后也能用到新建事项 / 复制 / 全状态切换等功能。**本版 `frontends` 仍未开启 mobile,长按代码作为预埋、未经真机验证;桌面端行为零改动(右键菜单 / 拖拽 / hover 全部不变)。**

### Added

- **长按触发菜单(预埋)**:新增 `domain/longpress.ts`(位移容差纯判定)+ `actions/longpress.ts`(Pointer Events action,仅 `pointerType==='touch'` 介入、按住 500ms 且位移不超 8px 触发,触发后吞掉随后的 click)+ `siyuan/menu.ts` 的 `openMenuForEvent` helper(桌面 `open({x,y})` / 移动端 `fullscreen` 优雅回退)。月视图日期格 / Dock 迷你日历 cell / 事项卡状态按钮三处接长按,复用现有右键 handler。
- `store/plugin.ts` 暴露 `setIsMobile` / `getIsMobile`,`index.ts` onload 按 `getFrontend` 写入。

### 桌面零副作用

- longpress action 首行 `pointerType !== 'touch'` 即 return,鼠标 / 笔 / 右键路径零接触;所有 `menu.open` 改走 `openMenuForEvent` 后桌面分支逐字等价;右键 `on:contextmenu` 全保留。

### 其它

- 测试 169 → **177**(+8:`exceedsMoveTolerance` 纯判定 5 / `openMenuForEvent` helper 3);spec+plan 见 `docs/superpowers/{specs,plans}/2026-05-29-mobile-longpress-menu*`

---

## [0.5.1] - 2026-05-29

v0.5.0 发版后用户反馈的节假日显示调整:换配色 + 「休/班」角标区分放假与调休,消除调休上班日"看着像放假"的歧义。三处统一(月视图大格子 / 顶部「本月节假日」栏 / Dock 迷你日历)。

### Changed

- **节假日配色换方案**:放假日改淡红(`--b3-card-error-color` 8%)、调休上班日改淡灰(`--b3-theme-on-surface-light` 10%),替换原暖橙(warning)/ 蓝(info)12% 配色。
- **月视图大格子加「休/班」角标**:放假日数字旁显示红色「休」、调休上班日显示灰色「班」,节日名保留;角标在左上(紧跟数字),与 v0.5.0 的右上角「+」新建按钮分离、互不遮挡。
- **调休不再标成放假**:顶部栏与 Dock chip 由 `劳动节 (调休)` 改为前缀式 `班 劳动节`(放假日 `休 劳动节`)。Dock 迷你日历圆 cell 太小放不下文字角标,改用数字色 + 底色区分(放假红 / 调休灰)。

### 其它

- 测试 166 → **169**(+3:DayCell 休/班角标用例);spec/效果图见 `docs/superpowers/{specs/2026-05-29-v0.5.1-holiday-display-design.md,mockups/2026-05-29-holiday-display.html}`

---

## [0.5.0] - 2026-05-29

月视图从 dot 聚合升级为**可拖拽的事项标题条**,支持拖拽改日期。同时引入统一的 Pointer Events 拖拽基础,为后续移动端适配预埋。

### Added

- **月视图事件条化**:`DayCell` 从 8px dot 聚合改为显示事项标题条,每格最多 3 条 + "+N 更多"。
- **拖拽改日期**:鼠标拖事项条到另一格即可修改该事项日期(`event.date`),经现有防抖持久化;支持拖到相邻月的补充格。落回原格或格子外不变更。拖拽中被拖条半透明 + 浮层跟手 + 目标格主色高亮。
- **格子新建按钮**:每个日期格右上角新增 "+" 按钮(桌面 hover 显示、触摸设备常显淡色),一键新建当日事项——补回事项条铺满格子后被挤压的新建入口。
- **Pointer Events 拖拽基础**(`actions/draggable.ts` + `domain/drag.ts`):一套指针逻辑统一鼠标(移动 5px 阈值触发)与触摸(长按 400ms 触发、命中前划动让位页面滚动),零运行时依赖。

### Changed

- 月视图点击语义明确:点事项条 = 打开编辑弹窗;点格子空白 = 当日事项详情;右键 = 菜单(均保留)。
- 重复事项实例在月视图**不可拖拽**(`not-allowed` 光标),仍可点击进编辑弹窗改期——避开"拖拽 × 重复 scope"的语义歧义。

### 已知限制

- **移动端触摸拖拽尚未经真机验证**:plugin.json `frontends` 本版仍为 `desktop`,**未开启 `mobile`**——整个插件在思源移动端从未验证,本版不贸然声称移动端支持。触摸拖拽逻辑已实现,作为后续整体移动适配的基础;待移动端整体验证通过后再开启 `mobile` frontend。

### 其它

- 测试 151 → **166**(+15:`drag` 纯函数 9 / `EventBar` 4 / `DayCell` 重写后净 +2)
- spec/plan 见 `docs/superpowers/{specs,plans}/2026-05-29-v0.5-drag*`

---

## [0.4.2] - 2026-05-29

v0.4.1 节假日色 token 化收口后的技术债清理。纯内部重构 + 废弃配置字段物理删除,**无新增用户可见功能**。

### Changed

- **配置加载改用白名单归一化**:新增 `domain/config.ts` 纯函数 `normalizeConfig`,`loadConfig` 改为经它处理原始存档。`holiday` 子对象深合并默认值后按合法 key 白名单过滤——旧存档残留的废弃 key 被自然剥离,顺带修复原浅合并对嵌套 `holiday` 整体替换、未来新增字段丢默认值的隐患。无启动主动重写存档,下次任意 `updateConfig` 落盘时自然写入干净版本。
- **`openSetting` 的 `holidayApp` 提升为实例字段**:对齐 `dockApp` 生命周期模式,`destroyCallback` 改可选链兜底,消除原闭包引用 `const` 声明的 TDZ 隐患(`HolidaySettings` 构造抛错时销毁不再 `ReferenceError`)。

### Removed

- **物理删除废弃配置字段** `Config.holiday.holidayBgColor` / `holidayWorkdayBgColor`(v0.4.1 已 silent deprecation,删 color picker + DayCell props),从 `types/config.ts` 接口与 `DEFAULT_HOLIDAY_CONFIG` 默认值移除。旧存档经 `normalizeConfig` 自动剥离,无需用户操作。

### Fixed

- **DayDetailDialog 节假日 chip 收口 v0.4.1 漏网写死色**:删除 inline `style="background: {holidayBgColor / holidayWorkdayBgColor}"` 与 CSS `color: #856404`,改用 `class:` 指令 + token 配色(节假日暖色 `--b3-card-warning-color` / 调休蓝色 `--b3-card-info-color` + `color-mix 12% alpha`),与 TodayList chip 一致。这是 v0.4.1 节假日 token 化时漏收的第 5 处。

### 其它

- 测试 146 → **151**(+5:`normalizeConfig` 单测)

---

## [0.4.1] - 2026-05-28

v0.4.0 之后的小幅 UX 调整。Dock 日历可跨日浏览 + CalendarView dot 不再因 status 消失 + EventEditor chip 语义色化。

### Added

- **Dock 日期跳转**:MiniCalendar 左键日期 → TodayList 切显该日事项;非 today 的选中日 cell 加 1.5px 主色 outline。mini 标题 "YYYY / MM" 改按钮,点击回今日(viewYear/Month 重置 + selectedDate=today)。
- **TodayList 加 `selectedDate` prop**(默认 today,向后兼容):selectedDate≠today 时,标题改「YYYY-MM-DD 周X」,隐藏过期分组,事项过滤到该日;selectedDate=today 时保留原有「今日 + 过期」分组。
- **SegmentedControl 加 `valueColors` prop**:可选地为每个 value 在 active 时填充语义色 + 白字;不传 prop 行为不变。
- 新增 14 个组件测试(TodayList ×5 / MiniCalendar ×5 / DayCell ×3 / SegmentedControl ×1)
- 测试基础设施:`src/__mocks__/siyuan.ts` + vitest alias,支持组件测试触发 siyuan 包 import
- 测试 132 → **146**

### Changed

- **CalendarView dot 默认全显示**:done/cancelled 事项的 dot 不再消失,以 0.2 alpha 显示(打完勾仍能看出"那天有过事项")。
- CalendarView 4 个 status chip 语义变更:从"过滤 dot 渲染"改为"控制 dot 透明度"。选中 → 该 status dot 100% alpha;未选 → 该 status dot 0.2 alpha。DayDetailDialog 仍显示所有事项,行为不变。
- **EventEditor 状态 / 优先级 chip 选中态语义色化**,跟 EventCard 状态圆 + DayCell dot 对齐:
  - 优先级:高=红 / 中=主色蓝 / 低=绿
  - 状态:进行中=主色蓝 / 已完成=绿 / 已取消=深灰 / 待办=中性默认(无色)

### Fixed

- DayCell 内 `.chx-day__dot--done` / `--cancel` 的 `opacity: 0.4` 死代码删除(被 filter 滤掉时根本不渲染,实际从未生效)。
- **EventCard `.chx-card__bar--low` 灰 → 绿一致化**:原 `var(--chx-separator)` 灰色,与 DayCell dot `--low=绿` 不一致(隐藏 bug);现统一 `var(--chx-success)`。
- **节假日色全 token 化(v0.4.0 未完代码收口)**:Dock MiniCalendar / 月视图 DayCell / CalendarView 节假日横幅 / HolidaySettings 四处节假日色全部走 `var(--b3-card-warning-color)` + `color-mix(... 12% alpha)`,消除 `#fff3cd`/`#d4edda`/`#856404`/`#155724` 写死色,深色主题下视觉不再崩。MiniCalendar 节假日 cell 首次显示 alpha 底色(跟月视图视觉一致)。HolidaySettings 配套移除两个 color picker(`Config.holidayBgColor` / `holidayWorkdayBgColor` 字段 silent deprecation)。

---

## [0.4.0] - 2026-05-28

v0.3 之后的视觉收口 + EventEditor date/time 输入 iOS 化。Dock 内 4 个 region 统一到 iOS Reminders 风,消除写死色,深色主题完整适配。

### Changed — UI 收口

- **MiniCalendar**:cell 透明背景 + 今日实心圆 + 周末灰 + 有事项 3px dot;节假日 / 调休改用 token 文字色(消除 `#fff3cd` / `#d4edda` 写死),深色主题完整适配
- **节假日横幅**:删除独立 region,合并为 MiniCalendar 下方 chip 行(`color-mix` alpha 背景)
- **过期警告**:删除独立 region,并入 TodayList 顶部「过期」分组,折叠/展开 chevron
- **TodayList**:重写为「过期 / 今日」双 GroupCard 分组;过期卡浅红 tint,今日卡中性灰 tint
- **EventEditor**:`<input type="date">` / `<input type="time">` 替换为自研 DatePicker / TimePicker(零依赖,复用 MiniCalendar 月格逻辑)

### Added

- 共享组件 `_shared/DatePicker.svelte` — iOS 风 mini date popover,`position: fixed` 逃出 Dialog overflow + 智能上下弹定位,4 个 TDD 测试
- 共享组件 `_shared/TimePicker.svelte` — hour / min 两段下拉(min 步长 5 分),3 个 TDD 测试
- 设计 token 扩展:alpha tint(`--chx-warning-tint-alpha` 等)+ popover(`--chx-popover-shadow/radius/z`)+ MiniCalendar 几何(`--chx-mini-dot-size/today-circle`)
- 测试 125 → **132**

### Fixed

- **节假日 / 调休 / 过期警告色写死**(`#fff3cd` / `#d4edda` / `#856404`)→ Dock 内全部 token 化,深色主题不再崩
- 月视图 DayCell / CalendarView 节假日色保留为「背景色」语义(决策 B),与 Dock「文字色」语义解耦,互不影响

### Compatibility

- 浅色 / 深色 / 用户自定义思源主题自动跟随(`color-mix` 兼容 Chromium 130+,思源最低版本 3.2.1 满足)
- 思源最低版本仍为 3.2.1
- `holidayBgColor` / `holidayWorkdayBgColor` 在月视图仍是背景色(自定义可生效);Dock 内由 token 接管(不再受 config 影响)

### Known limitations

- v0.1.0「Known limitations」剩余项(拖拽改日期 / 跨日事件 / 提醒 / 周日视图 / 多窗同步 / 反查事项 / 颜色自定义 / 自定义状态名 / 移动深度优化 / 导入导出)仍未做
- 新增 v0.4.x 候选(memory 中跟踪):MiniCalendar 日期点击跳转 / CalendarView done dot 默认可见

### Docs

- 设计 spec:[`docs/superpowers/specs/2026-05-28-v0.4.0-dock-redesign-design.md`](docs/superpowers/specs/2026-05-28-v0.4.0-dock-redesign-design.md)
- 实施计划:[`docs/superpowers/plans/2026-05-28-v0.4.0-dock-redesign.md`](docs/superpowers/plans/2026-05-28-v0.4.0-dock-redesign.md)

---

## [0.3.1] - 2026-05-27

发版元数据 patch — 修复占位 icon + 更精准的中文显示名。

### Changed

- **`displayName.zh_CN`**:「ChronicleX 编年史」→ **「ChronicleX任务管理」**(更直观反映插件用途)
- **`icon.png`**:从模板自带「160 X 160」灰色占位图替换为思源蓝渐变 + 白色「CX」(160×160,iOS 风圆角)

### Added

- `scripts/gen_icon.ps1` — PowerShell + C# inline 生成 icon 的脚本,后续需要 regenerate 可直接跑

---

## [0.3.0] - 2026-05-27

视觉重设计 — 把 EventCard 与 5 个高频 Dialog 按 iOS Reminders 极简风重做,配色全部跟随思源主题。

### Changed — UI 重设计

- **EventCard**:状态 checkbox 14px → **22px**(iOS HIG 触控);左边框 → **3px 短优先级条**;标签 chip 改主色透明 pill;关联文档 / 重复 icon 与 meta 行融合;状态 4 态视觉(空 / 同心圆 / 绿✓ / 灰×)
- **EventEditor**:6 个分组 inset 卡片替代密集表单;状态 / 优先级用 **SegmentedControl** 替代 chip;Dialog 可滚动(max-height 80vh)
- **ScopeSelector**:GroupCard + radio 行 + hint 描述(原本是 3 个朴素 button)
- **DayDetailDialog**:节假日变 chip 内嵌标题区;「+」按钮 28px → **32px** 圆主色;事项列表用 GroupCard 包装;空态加 CTA 按钮
- **HolidaySettings**:**3 个 GroupCard** 段(节假日 / 外观 / 缓存)+ 更新频率用 SegmentedControl
- **DocPicker**:iOS search input(灰底圆角 + 🔍 + clear ×)+ GroupCard 列表行 + chevron
- **常规设置段**(`openSetting`):也用 GroupCard 包装,与节假日段视觉统一

### Added

- 共享组件 `_shared/GroupCard.svelte`(分组 inset 容器)
- 共享组件 `_shared/SegmentedControl.svelte`(iOS 分段控件)
- 设计 token `src/styles/ios-tokens.scss`(11 个颜色 alias 全部跟随思源 `var(--b3-*)` + 13 个几何 token + 5 个字号)
- SegmentedControl 单测 × 3(`@testing-library/svelte` 4.0 + jsdom)
- 测试 117 → **120**

### Fixed

- **EventCard 删除按钮可见性**:从「微弱灰 × 几乎不见」改为「hover 浮现红色 22px ⊖ 圆形」(`@media (hover: none)` 移动端始终可见)
- **MiniCalendar 空白格 hover 全白**:`--b3-theme-background-lighter` 在多数思源主题未定义,fallback 到白底;改为 `var(--b3-list-hover)` 思源原生淡灰

### Compatibility

- 浅色 / 深色 / 用户自定义思源主题自动跟随(所有视觉 token 都是 `var(--b3-*)` alias)
- 思源最低版本仍为 3.2.1
- 移动端 hover 降级为始终可见删除按钮(swipe 列入 v4)

### Known limitations

v0.1.0 「Known limitations」剩余项(拖拽改日期 / 跨日事件 / 提醒 / 周日视图 / 多窗同步 / 反查事项 / 颜色自定义 / 自定义状态名 / 移动深度优化 / 导入导出)仍未做,v4 按优先级展开。

### Docs

- 设计 spec: [`docs/superpowers/specs/2026-05-26-ios-style-redesign-design.md`](docs/superpowers/specs/2026-05-26-ios-style-redesign-design.md)
- 实施计划: [`docs/superpowers/plans/2026-05-26-ios-style-redesign.md`](docs/superpowers/plans/2026-05-26-ios-style-redesign.md)

---

## [0.2.0] - 2026-05-26

v1 之后的第一个 minor 版本。三大新增 + 一组关键 fix。

### Added — 重复事项

- 频率支持:每天 / 每周 / 每月 / 每年
- 间隔 `interval`(每 N 天/周/月/年)
- 周/月/年级别的过滤:`byWeekDay` / `byMonthDay` / `byMonth` / `bySetPos`(如"每月第二个周二")
- 结束条件:`count`(重复 N 次,从 startDate 起全局计数)与 `until`(截止日期),可同时设置取先到者
- 修改 / 删除的 **范围选择**:`single`(仅此实例) / `future`(此日期及以后) / `all`(整个序列)
- 单次实例改动会建 exception,不影响母序列
- EventCard 显示重复事项 🔁 图标
- 新增 86 个单元测试(`recurrence.test.ts` + `events.test.ts` 重复事项分支)

### Added — 删除按钮

- EventCard 加删除按钮(所有显示卡片的入口:DockView / TodayList / CalendarView / DayDetailDialog)
- 删除重复事项时弹 ScopeSelector 选择范围
- 删除关联文档的事项时,**只清事项**,不影响思源文档

### Added — 中国法定节假日

- 数据源:[NateScarlet/holiday-cn](https://github.com/NateScarlet/holiday-cn) GitHub raw
- 本地缓存到 plugin storage(`chroniclex-holidays.json`),按年存储
- 自动更新策略:可配「每月 / 每年 / 手动」,30 天阈值自动重拉
- **9 个 UI 组件全部集成**:MiniCalendar / MonthGrid / DayCell / CalendarView / DockView / TodayList / DayDetailDialog / EventCard / 新增 HolidaySettings
- 节假日 / 调休上班分别配色(可在设置面板自定义)
- 月视图 + Dock 顶部显示当月节假日概览
- 独立「节假日设置」段,集成在原设置 Dialog 下半部

### Fixed

- **节假日数据 URL 错误**(实际永远 404 / 静默失败):上游仓库根目录直接是 `{year}.json`,不是 `master/json/{year}.json`。修正后正确拉取
- **`refreshHolidays` 假性成功**:数据为 null 时不再静默放弃,改为 throw,UI 的 `catch` 自动显示真正的失败 toast
- **`fetchHolidayData` 违反 domain 分层**:从 `src/domain/holiday.ts` 移至 `src/store/holidays.ts`,domain 现 100% 纯函数(符合 CLAUDE.md 分层约束)
- 修复设置页面回归(`f5a74a7` `84faedb`)
- 修复重复事项 `count` 语义(从 startDate 起全局计数,而非按当前 range 起算)
- 修复 weekly/monthly 路径忽略 `interval` 的问题
- 修复 `isInEffectiveRange` 中对 `untilEnd` 的引用应为 `rule.until`

### Changed

- 测试数量 45 → **117**(domain + store 双层全覆盖,新增 fetch URL 回归测试 + refresh failure signaling)

### Known limitations

v0.1.0 的 11 项推迟项中,**「重复事项」**已在本版本完成。剩余项见 [v0.1.0 changelog](#010---2026-05-25),v2 后续视优先级展开:

- 事项颜色自定义
- 拖拽改日期
- 跨日事件(startDate + endDate)
- 提醒 / 通知
- 周视图 / 日视图切换
- 多窗口实时同步
- 从思源文档反向查事项
- 自定义状态名
- 移动端深度优化
- 导入 / 导出

### Docs

- 新增 spec:[`docs/superpowers/specs/2026-05-26-recurrence-design.md`](docs/superpowers/specs/2026-05-26-recurrence-design.md)
- 新增 spec:[`docs/superpowers/specs/2026-05-26-delete-button-and-holiday-design.md`](docs/superpowers/specs/2026-05-26-delete-button-and-holiday-design.md)
- 对应 plan 同名于 [`docs/superpowers/plans/`](docs/superpowers/plans/)

---

## [0.1.0] - 2026-05-25

ChronicleX 首个公开版本。一个以日历为核心的「计划 + 回顾」插件,支持思源文档关联。

### Added — 主功能

**双入口**
- Dock 侧栏:迷你月历 + 今日清单 + 过期事项折叠横幅,常驻在思源右侧
- Tab 全屏月视图:7×6 月格,每日事项以彩色小点呈现(按优先级染色)
- 顶栏 📅 图标 + 命令面板快捷键 `⇧⌘L` 打开 Tab

**事项管理**
- 4 态状态模型:待办 / 进行中 / 已完成 / 已取消
- 3 级优先级:高 / 中 / 低
- 字段:标题、日期(必填)、可选时间段、短备注、多标签、可选关联文档
- 状态切换两种姿势:左键 icon = 一键完成/撤销;右键 icon = 全状态菜单
- 完成时自动记录 `completedAt`,便于后期复盘

**创建入口(4 种)**
- Dock 顶部 `+` 按钮 → 快速建今日事项
- Dock 迷你日历右键日子 → 菜单建事项
- Tab 月视图右键日子 → 菜单(新建 / 新建并立即建文档 / 复制此日某个事项)
- Tab 月视图左键日子 → 当日详情浮窗,内含 `+` 按钮

**思源文档关联**
- 一键建文档:首次弹笔记本选择菜单 + 默认路径,之后自动用配置
- 新文档头部自动预填事项元信息卡(日期 / 状态 / 优先级 / 标签 / 事项 ID)
- 绑定已有文档:全文搜索弹窗,250ms 防抖
- 解除绑定:仅清除关联,不删除思源文档
- 打开关联文档:点 📄 图标 → 校验文档存在 → 思源原生 openTab(失效会 toast 提示)

**搜索与筛选**
- 标题 + 备注模糊搜索
- 4 状态 chips(默认隐藏已完成 / 已取消)
- 标签 chips(自动从全部事项收集)
- 筛选条件 session 级,不持久化

**设置面板**
- 默认笔记本(下拉列出所有打开的笔记本)
- 默认文档路径(可输入,默认 `/ChronicleX`)
- 一周从哪天开始(周一 / 周日,默认周一)

**国际化**
- 简体中文(主)
- English

### Added — 技术亮点

- **轻量自研架构**:无外部日历库,Svelte 4 + CSS Grid 手写月格,bundle gzip 仅 ~50KB
- **分层清晰**:`types/` → `domain/`(纯函数) → `store/`(Svelte writable) → `siyuan/`(思源 API 适配层) → `components/`
- **45 个单元测试**:domain(id / date / search)+ store(plugin / events / config)全覆盖,Vitest
- **持久化**:思源 Plugin Data API,内存写入 + 500ms 防抖落盘;`onunload` 立即 flush 防丢失
- **Storage schema 版本号**:为未来迁移留口(`version: 1`)
- **思源同款 ID 格式**:`YYYYMMDDHHmmss-xxxxxxx`,与原生块 ID 一致
- **样式与思源主题深度集成**:全部使用 `var(--b3-*)` 变量,深浅色模式自动适配
- **思源 API 隔离**:所有思源运行时调用集中在 `src/siyuan/api.ts`,未来思源 API 变动只改一处

### Known limitations

以下功能 v1 未做,计划在 v2 考虑:

- 重复事项(每周 / 每月)
- 事项颜色自定义
- 拖拽改日期
- 跨日事件
- 提醒 / 通知
- 周视图 / 日视图切换
- 多窗口实时同步
- 从思源文档反向查事项
- 自定义状态名
- 移动端深度优化
- 导入 / 导出

### Compatibility

- 最低思源版本:**3.2.1**(`minAppVersion`)
- 主测平台:`desktop` / `desktop-window`
- 其他平台(mobile / browser-*)能加载但未优化

### Docs

- 完整设计文档:[`docs/superpowers/specs/2026-05-25-chroniclex-design.md`](docs/superpowers/specs/2026-05-25-chroniclex-design.md)
- 实施计划:[`docs/superpowers/plans/2026-05-25-chroniclex-v1.md`](docs/superpowers/plans/2026-05-25-chroniclex-v1.md)
- 开发指南:[`CLAUDE.md`](CLAUDE.md)
