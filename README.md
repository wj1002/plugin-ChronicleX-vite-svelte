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

- **v1.5.0** (2026-06-05): **Note-activity heatmap + month-view progress bars** — the Review overview gains a green "Note activity" heatmap that maps SiYuan document edit times onto a one-year calendar, forming a "plan vs. actual" contrast with the event "completion heatmap"; includes a notebook selector to filter. Month-view event bars become status progress slots (todo empty / doing half-filled / done full / cancelled gray-full + strikethrough, fill color follows priority), making each event's status readable at a glance. See [CHANGELOG](CHANGELOG.md). Tests 399 → 414
- **v1.4.0** (2026-06-04): **Kanban view + stability overhaul** — the Review page gains a "Kanban" mode that groups event cards into columns by status (todo / doing / done / cancelled) with drag-to-change-status across columns; plus a fix for an important data-correctness bug (Gregorian "yearly" recurrence was wrongly expanding to "daily", firing reminders every day), and a batch of fixes for recurrence robustness, resource leaks, reactive holiday loading and reminder performance; Review layout polish (Kanban fills height, overview full-width 2-column, heatmap centered). Removed ~830 lines of template dead code. See [CHANGELOG](CHANGELOG.md). Tests 376 → 399
- **v1.3.1** (2026-06-04): **Icon polish** — emoji in the Dock title / holiday chips / mobile action bar (`📅`/`🎉`/`🔍`/`⚙️`) all replaced with inline SVG icons (Lucide-style, `stroke` follows theme color); also fixed icons rendering as solid black blocks when SiYuan's global `svg{fill}` rule overrode them. Tests 372 → 376
- **v1.3.0** (2026-06-03): **Lunar recurring events** — events can recur every lunar year (birthdays / memorials / anniversaries), auto-landing on the correct Gregorian date each year. The recurrence editor shows a "农历" (lunar) checkbox under "yearly" plus a read-only confirm label "农历八月十五"; the anchor lunar month/day is auto-derived from the Gregorian start date. Leap months fire only in the regular month, day-30 clamps to the month's last day (廿九); reuses interval/count/until + this/future/all edit scope; reminders fire on the next Gregorian instance. Zero impact on existing solar recurrence. Tests 354 → 372
- **v1.2.0** (2026-06-03): **Multi-window sync (refresh on focus)** — on the same device, multiple SiYuan windows each load the plugin independently; when a window regains focus / becomes visible it reloads events from the kernel file so that window's calendar / Dock / day / week views auto-refresh to the latest. Events only; no WebSocket / polling / new dependency. Pure `eventsChanged` change-detection + `reloadEventsIfChanged` (a guard prevents clobbering local unsaved edits). Tests 343 → 354
- **v1.1.0** (2026-06-03): **Lunar calendar display** — lunar day + 24 solar terms + traditional festivals in month/week/day views (priority festival > solar-term > lunar-day, one shown; terms/festivals emphasized with accent color + bold); full lunar info in the day-view header; a "显示农历" settings toggle (default on). Powered by lunar-javascript, theme-following.
- **v1.0.1** (2026-06-03): **Holiday color redesign** — off-days switch from SiYuan error-red to warning amber/gold (very light tint + amber "休" badge emphasis + neutral holiday-name text, ending the red overload); make-up workdays stay neutral gray; unified across month view / Dock / top holiday bar, designed via ui-ux-pro-max, theme-following. Also: plugin.json funding field; README feedback link moved to top, release history trimmed.
- **v1.0.0** (2026-06-02): 🎉 **First stable release · full UI redesign (Soft UI + Bento)** — new form language: main calendar / editor / dialogs use Soft UI multi-layer soft shadows + stable hover; review dashboard / Dock use a Bento module grid (review widescreen: "distribution · overdue" side-by-side, "overview · heatmap" full-width); colors still alias SiYuan `var(--b3-*)` and follow light/dark theme. Font sizes fully tokenized (60+ sites), global keyboard focus ring + `prefers-reduced-motion`, dark-theme hardcoded colors fixed, aria-labels added to icon buttons. Tests held at 337
- **v0.11.0** (2026-06-02): **Reminders / notifications** — events fire a system desktop notification at their due time (falls back to an in-app SiYuan message if permission is unavailable); per-event lead time set in the editor (timed: on time / 5·15·30·60 min·1 day before; all-day: same day / 1·2 days before, anchored to a global "all-day reminder time", default 09:00); settings gain a "提醒" group (global toggle + all-day time). Clicking a notification focuses SiYuan and opens that event's editor; reminders missed while SiYuan was closed are merged into one catch-up notification on next launch. Done/cancelled events don't fire; recurring events only remind their next instance. Tests 294 → 320
- **v0.10.0** (2026-06-02): **Custom event colors** — a new "颜色" group in settings lets you pick a preset color (red/orange/yellow/green/blue/purple/pink/gray) for each of the high/normal/low priorities; clicking a swatch applies instantly. Event cards, month-view event bars, and the editor's priority control all follow, across month/week/day views and on mobile. Priority-level only (data model unchanged). Tests 276 → 294
- **v0.9.0** (2026-06-01): **Import / Export (JSON backup & migration)** — a "数据" group in settings: export all events to JSON, import from JSON (merge by id / replace with confirm), with per-event validation that skips invalid entries and reports counts, plus an empty-import guard against accidental wipe; pure functions in `domain/io.ts`, events-only, desktop-first
- **v0.8.1** (2026-06-01): **"Open" button for the linked doc** — the editor's "已绑定" row gains an "打开" button next to "解除" that opens the linked document tab via `openDoc` (keeps the editor dialog open so unsaved edits aren't lost; shows a message if the doc was deleted)
- **v0.8.0** (2026-06-01): **Month / Week / Day view switching** — toolbar segmented control, choice persisted in `config.view`; week view is 7 columns (reuses DayCell, cross-column drag-to-reschedule, per-column scroll), day view is a single-day list (reuses EventCard, inline status/delete/edit); navigation unified to an anchor date stepping per view with a view-aware title; week/day views expand recurring events. "Jump to calendar" now lands on the day view
> Earlier releases (v0.7 doc reverse-lookup, v0.6 mobile support & mobile-specific interactions, v0.5 drag-to-reschedule, v0.3–v0.4 iOS-style redesign, v0.2 recurring events / holidays, v0.1 base calendar) — see [CHANGELOG.md](CHANGELOG.md) below.

See [CHANGELOG.md](CHANGELOG.md) for full release notes.

### Deferred to future versions

- Multi-day events (startDate + endDate)

---

## ☕ Support

If you find ChronicleX helpful, feel free to buy me a coffee — thank you for your support!

![赞赏码](https://gitee.com/wj741208617/donate-page/raw/master/zs2.png)

---

## ⚖️ License

All Rights Reserved © Aether
