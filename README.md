# ChronicleX · SiYuan Calendar & Task Plugin

English | [简体中文](https://github.com/wj1002/plugin-ChronicleX-vite-svelte/blob/main/README_zh_CN.md)

A **calendar-based task manager** for SiYuan: log todos on the calendar, track their status, leave a trace when done, and **link a SiYuan document** for the details.

---

## ✨ Features

### 📅 Dual Entry Points
- **Dock Sidebar**: Always-on right panel with mini calendar + selected-day list (left-click a mini cell to browse across days) + collapsible overdue group
- **Tab Full-Screen Month View**: 7×6 grid (42 cells) with per-day **event title bars** (priority-colored left edge; up to 3 per cell + "+N"; done/cancelled dimmed but visible)
- **Top-Bar Entry + Shortcut**: Click the 📅 top-bar icon to open the full-screen Tab; or trigger the command via `⇧⌘L` / `Shift+Ctrl+L`

### 🖐️ Drag & Quick Actions
- **Drag to reschedule**: In month view, drag an event bar to another cell to change its date — unified mouse (desktop) + touch (long-press); recurring instances are not draggable (use the edit dialog)
- **Quick new-event button**: Each day cell shows a `+` on hover (top-right) to create an event on that day
- **Click bar to edit / click blank for details**: Clicking a bar opens the editor; clicking the cell's blank area opens the day list

### 📱 Mobile Support
- **Runs on SiYuan mobile**: every desktop-only interaction has a touch path added, usable on the SiYuan mobile App / mobile browser (since v0.6.0)
- **Long-press menu**: long-press a month-view day cell / mini-calendar cell / status button to open the same menu as desktop right-click (create / copy / full status switch)
- **Swipe to delete**: iOS-style swipe-left on an event card reveals a delete action (swipe back to cancel)
- **Bottom action bar + FAB**: a fixed bottom bar (today / search / filter) plus a bottom-right "＋" new-event FAB; the top filters move into a bottom sheet (v0.6.3)
- **Drag to reorder**: in the day-detail dialog's sort mode, hold the ≡ handle to reorder that day's events, persisted (v0.6.4; recurring events sink to the bottom, not draggable)
- **Narrow-screen dialog fit**: editor / doc picker / day detail / scope selector dialogs adapt their width on narrow phones without overflowing (v0.6.5)
- **Narrow-screen layout polish**: on phones the month grid is denser (less padding, shorter cells), settings rows stack vertically with full-width controls, and the editor's time range wraps to its own full-width line (v0.6.6)

### ✅ Event Management
- **4-State Status**: todo / doing / done / cancelled
- **3 Priority Levels**: high / normal / low — event bar's left edge is priority-colored
- **Multi-Tag**: Free-form tags, auto-aggregated into filterable chips
- **Status Toggle**: Left-click icon to toggle complete, right-click for full state menu
- **Auto-Record**: `completedAt` timestamp on completion for review

### 🔗 SiYuan Document Linking (Headline Feature)
- **One-Click Create Doc**: Pick default notebook on first use, auto-used thereafter
- **Auto-Prefill**: New docs filled with event meta block (date / status / priority / tags / ID)
- **Bind Existing Doc**: Full-text search picker with 250ms debounce
- **Unbind**: Only clears association, doesn't delete SiYuan doc
- **Open Linked Doc**: Click 📄 to jump (toast on missing/deleted)
- **Reverse lookup (v0.7.0, trigger fixed in v0.7.2)**: from a SiYuan **doc-tree right-click menu**, open "ChronicleX 关联事项 (N)" to list events linking to that doc; each row can edit the event or jump to its date on the calendar

### 🔁 Recurring Events
- **Frequency**: Daily / Weekly / Monthly / Yearly
- **Interval**: Every N days/weeks/months/years
- **Filters**: By weekday (weekly) / by month day (monthly)
- **End Conditions**: Never / Repeat N times / Until date
- **Scope Editing**: Single instance / this date onward / entire sequence

### 🎨 UI Design
- **EventCard**: Priority bar + tag pills + 4-state visuals
- **EventEditor**: Grouped cards + SegmentedControl
- **DayDetailDialog**: Holiday chips + empty-state CTA button
- **DocPicker**: Search input + GroupCard list
- **Theme Integration**: All visual tokens use `var(--b3-*)` variables, auto light/dark

### 🎉 Chinese Public Holidays
- **Source**: NateScarlet/holiday-cn
- **Local Cache**: Per-year storage in plugin storage
- **Auto Update**: Configurable monthly/yearly/manual, 30-day threshold
- **休/班 (off/work) distinction**: Month cells show a 休 (day-off, red) / 班 (make-up workday, gray) badge next to the date + holiday name; the holiday bar and Dock chips use `休 …` / `班 …` prefixes so make-up workdays aren't shown like a holiday
- **Colors**: day-off **soft red** / make-up workday **soft gray**, fully theme-tokenized for light/dark
- **Next-holiday countdown**: the month-view top bar shows "N days until {holiday}", or "{holiday} · on holiday" when today is an off day
- **UI Integration**: Dock mini calendar / month-view DayCell / month-view banner / DayDetailDialog in sync
- **Toggles**: Global on/off / show label / show holiday name

### 🔍 Search & Filter
- **Keyword Search**: Title + notes fuzzy match (month-view toolbar)
- **Status Filter**: 4-state chips control event-bar/dot opacity — selected at 100%, unselected dimmed (not hidden — you can still see "that day had something")
- **Tag Filter**: Auto-collected from all events
- **Day Detail Popover**: Left-click a cell's **blank area** → event list → edit/create
- **Bind-Doc Search** (DocPicker): Title/full-text search with 250ms debounce

### 🗑️ Delete Function
- **Desktop**: red circular `−` button appears on card hover
- **Mobile**: iOS-style **swipe-left** on a card reveals the delete action (swipe back to cancel)
- **Delete Recurring**: Scope selector for range
- **Delete Linked Event**: Only clears event, preserves SiYuan doc

---

## 📥 Installation

1. Download the latest `package.zip` from [Releases](https://github.com/wj1002/plugin-ChronicleX-vite-svelte/releases)
2. Unzip into `{SiYuan workspace}/data/plugins/plugin-ChronicleX-vite-svelte/`
3. Restart SiYuan, enable via Settings → Marketplace → Downloaded

---

## 🚀 Quick Start

> 📱 Mobile: replace every "right-click" below with "long-press", and delete by swiping a card left.

Once enabled:

1. **Right-side Dock** shows 📅 ChronicleX with mini calendar + today list
2. **Create event**:
   - Dock `+` button → quick today entry
   - Right-click any day → menu with create options
   - Top-right `+` on a cell → quick new event on that day; or left-click the cell's blank area → day detail popover → `+` inside
3. **Reschedule**: In month view, just **drag an event bar** to the target cell (recurring events: use the edit dialog)
4. **Open full view**: Click 📅 / press `⇧⌘L` / `Shift+Ctrl+L`
5. **Link doc**: Click `+ Create doc` or `⊕ Bind existing doc` in editor
6. **Toggle status**: Left-click `○` → `●`; right-click `○` → 4-state menu
7. **Filter**: Toolbar chips + search box

---

## ⚙️ Settings

Settings → Marketplace → Downloaded → ChronicleX → gear:

| Setting | Default | Description |
|---|---|---|
| Default notebook | (unset) | Where "create doc" stores generated docs |
| Default doc path | `/ChronicleX` | Relative path inside the notebook |
| Week starts on | Monday | First column of the calendar |
| Priority colors | red/blue/green | Pick a preset color per high/normal/low; event bars and editor follow in real time |
| Enable reminders | On | Master switch for due-time system notifications |
| All-day reminder time | 09:00 | Anchor time used to remind events that have no specific time |
| Show holidays | On | Display Chinese public holidays |
| Holiday update | Monthly | Auto-fetch frequency |
| Show label / Show name | On | Toggle the 休/班 (off/work) badge and the holiday name on month cells |

---

## 📋 Compatibility

- Minimum SiYuan version: **3.2.1**
- **Desktop**: Windows / macOS / Linux
- **Mobile**: SiYuan mobile App + mobile browser (supported since v0.6.0, adapted with touch interactions — long-press menu / swipe-to-delete / touch drag)

---

## 🗺️ Release History & Roadmap

### Released

- **v0.11.0** (2026-06-02): **Reminders / notifications** — events fire a system desktop notification at their due time (falls back to an in-app SiYuan message if permission is unavailable); per-event lead time set in the editor (timed: on time / 5·15·30·60 min·1 day before; all-day: same day / 1·2 days before, anchored to a global "all-day reminder time", default 09:00); settings gain a "提醒" group (global toggle + all-day time). Clicking a notification focuses SiYuan and opens that event's editor; reminders missed while SiYuan was closed are merged into one catch-up notification on next launch. Done/cancelled events don't fire; recurring events only remind their next instance. Tests 294 → 320
- **v0.10.0** (2026-06-02): **Custom event colors** — a new "颜色" group in settings lets you pick a preset color (red/orange/yellow/green/blue/purple/pink/gray) for each of the high/normal/low priorities; clicking a swatch applies instantly. Event cards, month-view event bars, and the editor's priority control all follow, across month/week/day views and on mobile. Priority-level only (data model unchanged). Tests 276 → 294
- **v0.9.0** (2026-06-01): **Import / Export (JSON backup & migration)** — a "数据" group in settings: export all events to JSON, import from JSON (merge by id / replace with confirm), with per-event validation that skips invalid entries and reports counts, plus an empty-import guard against accidental wipe; pure functions in `domain/io.ts`, events-only, desktop-first
- **v0.8.1** (2026-06-01): **"Open" button for the linked doc** — the editor's "已绑定" row gains an "打开" button next to "解除" that opens the linked document tab via `openDoc` (keeps the editor dialog open so unsaved edits aren't lost; shows a message if the doc was deleted)
- **v0.8.0** (2026-06-01): **Month / Week / Day view switching** — toolbar segmented control, choice persisted in `config.view`; week view is 7 columns (reuses DayCell, cross-column drag-to-reschedule, per-column scroll), day view is a single-day list (reuses EventCard, inline status/delete/edit); navigation unified to an anchor date stepping per view with a view-aware title; week/day views expand recurring events. "Jump to calendar" now lands on the day view
- **v0.7.2** (2026-06-01): **Fix** — reverse-lookup menu item never appeared: v0.7.0 hooked `click-editortitleicon`, but recent SiYuan opens the emoji/icon picker (not a menu) on title-icon click. Rewired to `open-menu-doctree` (**doc-tree right-click menu**), reading docId from the clicked node's `data-node-id`, injected only for a single `doc`
- **v0.7.1** (2026-06-01): **Fix** — long event titles no longer overflow and stretch their month-grid column (added `min-width:0` along the DayCell/EventBar grid+flex chain so titles truncate with `…` and column widths stay fixed)
- **v0.7.0** (2026-06-01): **Reverse lookup: doc → events** — a SiYuan document's title-icon menu now shows "ChronicleX 关联事项 (N)", listing events whose `linkedDocId` points at that doc; each row can edit the event or jump to its date on the calendar (via a `focusDateStore` signal that works for both new and already-open calendar tabs). Completes the doc↔event bidirectional link. Tests 240 → 246
- **v0.6.6** (2026-06-01): **Narrow-screen layout polish** — three mobile-only internal layout refinements: denser month grid (less padding/gap, `min-height` 90→60px, hidden holiday names yielding to the 休/班 badge), vertically-stacked settings rows with full-width controls, and the editor's time range wrapping to its own full-width line. All gated by `@media (pointer: coarse)` / `display:contents`, zero desktop side effects. Tests 237 → 240
- **v0.6.5** (2026-05-31): **Narrow-screen dialog fit** — mobile dialogs (editor / doc picker / day detail / scope selector) now use a unified `92vw` container and `min(Xpx, 100%)` root min-width, no longer overflowing on narrow screens; one-point fix in `simpleDialog`, zero desktop side effects. Tests 233 → 237
- **v0.6.4** (2026-05-31): Mobile **drag-to-reorder** — in the day-detail dialog, enter "sort mode" and drag the ≡ handle to reorder that day's events (fully manual, persisted; recurring events are not draggable and sink to the bottom); mobile-only, zero desktop side effects. Tests 214 → 233
- **v0.6.3** (2026-05-31): Mobile **bottom action bar + FAB** — a fixed bottom bar (today / search / filter) plus a bottom-right "＋" FAB; the top search / status / tag filters move into a bottom sheet; zero desktop side effects. Tests 200 → 214
- **v0.6.2** (2026-05-30): **Next-holiday countdown** in the month-view top bar — "N days until {holiday}", or "{holiday} · on holiday" when today is an off day; display-only, works on desktop and mobile.
- **v0.6.1** (2026-05-30): Mobile **swipe-to-delete** — iOS-style swipe-left on an event card reveals a red delete action (tap to delete / swipe back to cancel — natural mis-tap protection), replacing the always-visible `−` button on mobile; zero desktop side effects (hover `−` unchanged). Tests 177 → 189
- **v0.6.0** (2026-05-30): **Mobile enabled for the first time** — `frontends` now includes `mobile` / `browser-mobile`, so ChronicleX officially runs on SiYuan mobile; device walkthrough (via mobile browser to desktop SiYuan) passed with no functional blockers — core flows (month view / today list / create-edit / doc linking / holidays / long-press menu / drag-reschedule) all reachable and usable; settings dialog width adapts on narrow screens. Desktop unchanged. Narrow-screen layout polish and mobile-specific interactions deferred to later releases.
- **v0.5.2** (2026-05-30): Mobile long-press menu adaptation (groundwork) — adds a touch **long-press** trigger path alongside the desktop right-click menu (month-view day cells / Dock mini-calendar / event-card status button); a 500ms long-press opens the same menu as right-click. This release still keeps `frontends` desktop-only (mobile not enabled, not device-verified) — long-press ships as groundwork; **desktop behavior is unchanged** (right-click / drag / hover all intact). Tests 169 → 177
- **v0.5.1** (2026-05-29): Holiday display refinements — recolored holidays/make-up workdays (soft red / soft gray), added 休/班 (off/work) badges next to the date in month view (holiday name kept), make-up workdays no longer labeled like a holiday (`班` prefix instead of "(调休)"); unified across month cells, the holiday bar, and the Dock mini-calendar. Tests 166 → 169
- **v0.5.0** (2026-05-29): Month-view dots upgraded to **draggable event title bars** — drag to reschedule (desktop); each day cell gets a quick "+" new-event button; introduces a unified Pointer Events drag foundation. Touch-drag logic is implemented but this release keeps `frontends` desktop-only (mobile not enabled, not device-verified) — deferred to a future overall mobile adaptation. Tests 151 → 166
- **v0.4.2** (2026-05-29): Tech-debt cleanup — holiday config now normalized via a key whitelist (legacy deprecated fields auto-stripped), deprecated config fields physically removed, settings-panel lifecycle refactored, DayDetailDialog holiday chip tokenized. **No new user-facing features**; tests 146 → 151
- **v0.4.1** (2026-05-28)
  - Dock mini-calendar **click-to-jump**: click any day → today list switches to that day's events; selected non-today cells get a 1.5px primary outline ring; click the YYYY/MM header to jump back to today
  - Month-view dots always visible: done/cancelled event dots no longer disappear — shown at low alpha so you can still see "that day had something" after checking off; filter chips switched from "filter dots" to "control dot opacity"
  - Event editor **semantic-colored status & priority chips**: high=red, normal=blue, low=green; doing=blue, done=green, cancelled=gray — aligned with calendar dots and card status circles
  - Holiday color fully theme-tokenized: mini-calendar shows holiday tint for the first time; month-view holiday/workday colors no longer hard-coded (clean in dark theme)
  - Mini-calendar **larger geometry**: cell number 10→12px, today/selected circle 18→22px for better readability in narrow docks
- **v0.4.0** (2026-05-28): Dock 4 regions unified into iOS Reminders style; event editor date/time inputs replaced with custom DatePicker/TimePicker (zero-dep, iOS-style); tests 125→132
- **v0.3.0**: iOS-style UI full redesign (EventCard / EventEditor / DayDetailDialog and 6 other components)
- **v0.2.0**: Recurring events / delete button / Chinese public holidays
- **v0.1.0**: Base calendar + SiYuan doc linking

See [CHANGELOG.md](CHANGELOG.md) for full release notes.

### Deferred to future versions

- Multi-day events (startDate + endDate)
- Multi-window sync
- Custom status names

---

## ☕ Support

If you find ChronicleX helpful, feel free to buy me a coffee — thank you for your support!

![赞赏码](https://gitee.com/wj741208617/donate-page/raw/master/zs2.png)

---

## ⚖️ License

All Rights Reserved © Aether
