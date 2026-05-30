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

- Custom event colors
- Multi-day events (startDate + endDate)
- Reminders / notifications
- Week view / day view
- Multi-window sync
- Reverse doc-to-event lookup
- Custom status names
- Import / export

---

## ⚖️ License

All Rights Reserved © Aether
