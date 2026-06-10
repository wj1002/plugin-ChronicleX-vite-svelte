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
- **One-Click Create Doc**: Pick default notebook on first use, auto-used thereafter; doc path supports template variables for auto date-based filing
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
| Default doc path | `/ChronicleX` | Relative path inside the notebook, supports template variables: `{{date}}` `{{year}}` `{{month}}` `{{day}}` `{{title}}` and `{{now | date "YYYY-MM-DD"}}` formatting |
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

- **v1.8.1** (2026-06-10): **Doc path template variables** — the "Default doc path" setting now supports template variables for automatic date-based filing. Supports `{{date}}` `{{year}}` `{{month}}` `{{day}}` `{{title}}` simple variables and `{{now | date "YYYY-MM-DD"}}` custom date formatting pipes; paths are auto-normalized. See [CHANGELOG](CHANGELOG.md). Tests 540 → 555
- **v1.8.0** (2026-06-10): **Time-distribution proportion donut** — the Review overview gains a "Time distribution" donut card (conic-gradient) that shows event composition by **count proportion**, answering "where did this period's events cluster". A single ring with tabs to switch between three dimensions — **status / priority / tag** — following the existing week / month / year / all selector at the top. Status and priority show all fixed segments (including 0% ones); the tag dimension aggregates the Top 8 by occurrence with the rest folded into an "Other" segment, showing only non-zero ones. The ring center shows the total plus a unit ("items" for status/priority, "times" for tag), and each legend row shows count + percentage. Complements the existing "Task distribution" horizontal bars (absolute count) with a proportion view. See [CHANGELOG](CHANGELOG.md). Tests 526 → 540
- **v1.7.1** (2026-06-09): **Fix — statutory holiday & observance now shown together** — previously when a day was both a statutory holiday (e.g. a Dragon-Boat make-up off-day) and an observance (e.g. Father's Day, the 3rd Sunday of June), the statutory one won and the observance was dropped, so Jun 21 showed only "Summer Solstice + Dragon Boat Festival" and Father's Day was missing — inconsistent with the top "holidays this month" bar. Now both render: the statutory one keeps its off/work badge + background + name, the observance shows on its own line. Month and week views both fixed. See [CHANGELOG](CHANGELOG.md). Tests 511 → 526

> Earlier releases (v1.7 anniversaries/countdowns, v1.6 release-notes popup, v1.5 note-heatmap/progress bars, v1.4 kanban, v1.3 icon polish / lunar recurrence, v1.2 multi-window sync, v1.1 lunar display, v1.0 stable UI redesign, v0.8–v0.11 week-day views / reminders / custom colors / import-export, v0.7 doc reverse-lookup, v0.6 mobile support, v0.5 drag-to-reschedule, v0.3–v0.4 iOS-style redesign, v0.2 recurring events / holidays, v0.1 base calendar) — see [CHANGELOG.md](CHANGELOG.md).

### Deferred to future versions

- Multi-day events (startDate + endDate)

---

## ☕ Support

If you find ChronicleX helpful, feel free to buy me a coffee — thank you for your support!

![赞赏码](https://gitee.com/wj741208617/donate-page/raw/master/zs2.png)

---

## ⚖️ License

All Rights Reserved © Aether
