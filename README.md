# Ledger Expense Tracker

Ledger is a mobile-first, single-page expense tracker built with plain HTML, CSS, and JavaScript.
It is deploy-ready as a static app and includes full PWA support.

## Complete Feature List

### Transaction Management

- Add transactions with description, amount, category, date, and payment method
- Edit existing transactions
- Inline Updated badge after edit confirmation
- Delete transactions with 5-second Undo
- Search transactions by text
- Filter by category and payment method
- Sort by date or amount (ascending and descending)
- Month-based transaction scoping
- Recurring-generated transaction tagging in the table

### Recurring Expenses

- Create monthly recurring templates
- Set recurring day of month
- Set category and payment method per recurring template
- Auto-apply recurring templates once per month
- Pause and resume recurring templates
- Skip this month per recurring template
- Skip this month removes already generated recurring items for that template in the current month

### Budgeting

- Monthly global budget setting
- Budget usage percentage and over-budget detection
- Remaining budget calculations
- Category budget limits (optional per category)
- Category budget progress bars with warn and over states

### Dashboard

- Hero summary for current month spending
- Quick metrics for transaction count and daily pace
- Summary cards for total spent, budget, remaining, and daily average
- Category budget tracker card
- Recent transactions panel
- Spending by category doughnut chart
- Last 6 months trend chart

### Analytics Page

- Top analytics cards (total, average, top category, daily average, budget usage, largest expense)
- Category breakdown horizontal bar chart
- Daily spending line chart
- 12-month overview chart

### Category Management

- Add custom categories with icon and color
- Delete categories
- Category filters auto-refresh across pages
- Category references in recurring templates auto-correct if deleted

### Backup and Data Management

- Export all app data as JSON
- Export month data as CSV
- Export printable PDF report for current month
- Import JSON backup with confirmation
- Quick Snapshot file export
- Local Backup Center with rolling local snapshots (max 10)
- Backup Center actions:
	- Create local snapshot
	- Restore snapshot
	- Download snapshot
	- Delete snapshot
	- Clear snapshot history
- Auto snapshots triggered on saves with debounce and throttle
- Full data reset with confirmation

### PWA and Offline

- Installable web app manifest
- Service Worker app shell caching
- Offline caching for local assets and Chart.js CDN asset
- In-app update banner when a newer Service Worker is ready
- Update now flow using skip waiting and controlled refresh

### UX and Accessibility

- Mobile app shell layout with bottom tab navigation
- Animated active tab indicator
- Page transitions and staged chart reveals
- Keyboard shortcuts:
	- 1 to 5 for page switching
	- Left and Right arrow for month navigation
	- Escape to close confirmation modal
- Focus-visible outlines for keyboard users
- ARIA labels on key interactive controls
- Live-region toasts and undo notifications

## Data Storage Model

Ledger stores all data locally in browser storage.

Primary localStorage keys:

- ledger_v1
- ledger_snapshots_v1
- ledger_last_snapshot_at

Stored main state schema in ledger_v1:

- version: number
- budget: number
- categoryBudgets: object where keys are category names and values are limits
- categories: array of category objects
	- id, name, color, icon
- recurring: array of recurring template objects
	- id, description, amount, category, payment, day, active, appliedMonths
- expenses: array of expense objects
	- id, description, amount, category, payment, date, createdAt
	- optional: updatedAt, recurringId, recurringInstance

Important behavior:

- Data never leaves the device unless the user exports it manually.
- Clearing browser site data removes Ledger data.
- Private or incognito browser modes may block persistent localStorage.
- Import replaces existing data after confirmation.

## JavaScript Function Map (index.html)

This is the complete top-level function inventory in the app.

### Storage and schema

- isStorageAvailable
- normalizeState
- loadData
- getDefaultData
- saveData

### General helpers

- uid
- fmt
- fmtDate
- escHtml
- isChartReady
- getCatByName
- getMonthExpenses
- getSafeCategoryName
- monthKey
- getDateForMonth

### Recurring scheduling and add form state

- applyRecurringForMonth
- resetAddForm
- setAddMode

### Navigation and motion

- setActiveTabIndicator
- animatePageEntrance
- revealCharts
- revealDashboardSections
- showPage
- changeMonth
- updateMonthLabel
- updateSidebar

### Dashboard and analytics rendering

- renderDashboard
- renderCategoryBudgetSummary
- renderDonut
- renderTrend
- renderRecent
- renderExpenseRow
- renderTransactions
- renderAnalytics

### Filter and select population

- populateCatFilters
- populateAddCatSelect
- populateRecurringCatSelect

### Transaction CRUD

- addExpense
- startEditExpense
- cancelEditExpense
- deleteExpense
- queueUndoDelete
- clearPendingUndo
- markExpenseEdited
- queueAutoSnapshot
- undoDeleteExpense

### Settings and budgets

- renderSettings
- updateStorageStatusLine
- saveBudget
- renderCatList
- renderCategoryBudgetList
- saveCategoryBudgetById
- saveCategoryBudget

### Recurring template management

- renderRecurringList
- addRecurring
- toggleRecurring
- skipRecurringThisMonth
- deleteRecurring

### Category management

- addCategory
- deleteCategory

### Storage usage and exports

- updateStorageBar
- exportCSV
- exportPDF
- exportJSON

### Backup Center

- getSnapshotStore
- persistSnapshotStore
- createLocalSnapshot
- updateSnapshotMeta
- renderBackupCenter
- restoreSnapshot
- downloadSnapshot
- deleteSnapshot
- clearSnapshotHistory
- createSnapshotBackup

### Import, reset, and utilities

- importJSON
- confirmClearAll
- download

### Modal and toast

- openModal
- closeModal
- showToast

### PWA update flow

- showUpdateBanner
- hideUpdateBanner
- applyAppUpdate
- bindServiceWorkerUpdates
- registerServiceWorker

### App startup

- init

## Project Structure

- index.html: Full application UI, styles, and JavaScript logic
- manifest.webmanifest: PWA manifest
- service-worker.js: Cache and update lifecycle
- icons/: PWA and mobile icon set

## Run Locally

Use a static server (required for Service Worker and full PWA behavior):

- Python: python -m http.server 8080
- Node: npx serve .

Open:

- http://localhost:8080

## Deploy

Deploy as a static site to:

- GitHub Pages
- Netlify
- Vercel (static)
- Cloudflare Pages

Deployment requirements:

- Serve over HTTPS (or localhost for development)
- Keep all files in the same root path
- Keep manifest and Service Worker files enabled
