# ChronicleX · SiYuan Calendar & Task Plugin

English | [简体中文](https://github.com/wj1002/plugin-ChronicleX-vite-svelte/blob/main/README_zh_CN.md)

A **calendar-centered planner** for SiYuan: right-click any day to log a task, track its status, and **link it to a SiYuan document** for detailed notes.

---

## ✨ Features

### 📅 Dual Entry Points
- **Dock Sidebar**: Always-on right panel with mini calendar + today list + overdue banner
- **Tab Full-Screen Month View**: 7×6 grid with colored dots per day (by priority)
- **Top Bar Shortcut**: Click 📅 icon or press `⇧⌘L` / `Shift+Ctrl+L`

### ✅ Event Management
- **4-State Status**: todo / doing / done / cancelled
- **3 Priority Levels**: high / normal / low — colored dots in month cells
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
- **Filters**: By weekday / by month day / by month / by set position (e.g., "2nd Tuesday each month")
- **End Conditions**: Repeat N times OR until date
- **Scope Editing**: Single instance / this date onward / entire sequence

### 🎨 iOS-Style UI
- **EventCard**: Priority bar + tag pills + 4-state visuals
- **EventEditor**: Grouped cards + SegmentedControl
- **DayDetailDialog**: Holiday chips + empty-state CTA button
- **DocPicker**: iOS search input + GroupCard list
- **Theme Integration**: All visual tokens use `var(--b3-*)` variables, auto light/dark

### 🎉 Chinese Public Holidays
- **Source**: NateScarlet/holiday-cn
- **Local Cache**: Per-year storage in plugin storage
- **Auto Update**: Configurable monthly/yearly/manual, 30-day threshold
- **UI Integration**: 9 components including mini calendar, month view, dock, detail dialog
- **Custom Settings**: Separate colors for holidays/compensatory workdays, toggleable

### 🔍 Search & Filter
- **Keyword Search**: Title + notes fuzzy match with 250ms debounce
- **Status Filter**: 4-state chips, defaults hide done/cancelled
- **Tag Filter**: Auto-collected from all events
- **Day Detail Popover**: Left-click any day → event list → edit/create

### 🗑️ Delete Function
- **Delete Button**: Red circle on hover (always visible on mobile)
- **Delete Recurring**: Scope selector for range
- **Delete Linked Event**: Only clears event, preserves SiYuan doc

---

## 📥 Installation

1. Download the latest `package.zip` from [Releases](https://github.com/wj1002/plugin-ChronicleX-vite-svelte/releases)
2. Unzip into `{SiYuan workspace}/data/plugins/plugin-ChronicleX-vite-svelte/`
3. Restart SiYuan, enable via Settings → Marketplace → Downloaded

---

## 🚀 Quick Start

Once enabled:

1. **Right-side Dock** shows 📅 ChronicleX with mini calendar + today list
2. **Create event**:
   - Dock `+` button → quick today entry
   - Right-click any day → menu with create options
   - Left-click any day → day detail popover → `+` inside
3. **Open full view**: Click 📅 / press `⇧⌘L` / `Shift+Ctrl+L`
4. **Link doc**: Click `+ Create doc` or `⊕ Bind existing doc` in editor
5. **Toggle status**: Left-click `○` → `●`; right-click `○` → 4-state menu
6. **Filter**: Toolbar chips + search box

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
| Holiday colors | Red/blue | Customizable for holidays/compensatory workdays |

---

## 📋 Compatibility

- Minimum SiYuan version: **3.2.1**
- Primary tested: Windows / macOS / Linux desktop
- Other platforms: desktop-window / mobile / browser-desktop / browser-mobile
- Mobile: loads but not deeply optimized (swipe deferred to v4)

---

## 🗺️ Roadmap

Completed: v0.1.0 base → v0.2.0 recurrence/delete/holidays → v0.3.0 iOS UI

Deferred to future versions:

- Custom event colors
- Drag to reschedule
- Multi-day events (startDate + endDate)
- Reminders / notifications
- Week view / day view
- Multi-window sync
- Reverse doc-to-event lookup
- Custom status names
- Mobile deep optimization
- Import / export

---

## ⚖️ License

MIT © Aether
