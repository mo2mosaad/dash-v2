# Words Tie — Dashboard Theme · Integration Guide

This reskins the **entire** dashboard to match words-tie.com without touching
your Blade logic, routes, loops, CSRF tokens or controllers. It works by
overriding the Bootstrap/Sneat component styles globally — so every page
(tasks, clients, freelancers, finance, forms, modals, tabs…) updates at once.

There are only **3 required steps**.

---

## Step 1 — Add the theme stylesheet

Copy `wordstie-theme.css` into your project here:

```
public/assets/dashboard/assets/css/wordstie-theme.css
```

## Step 2 — Link it in `head.blade.php`  (resources/views/dashboard/layouts/head.blade.php)

**(a)** Add the brand fonts. Right after the existing Public Sans `<link>`, add:

```html
<link
  href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&family=JetBrains+Mono:wght@500;700&display=swap"
  rel="stylesheet" />
```

**(b)** Link the theme **last** so it overrides the template. Place this just
before the `<!-- Helpers -->` comment (i.e. after the `cards-advance.css` line):

```html
<!-- Words Tie brand theme (must load AFTER the template CSS) -->
<link rel="stylesheet" href="{{ asset('assets/dashboard/assets/css/wordstie-theme.css') }}" />
```

**(c) Optional cleanup** — in the same file there's an inline `<style>` block
("Custom sidebar styling") that hard-codes the old navy/blue sidebar
(`#232E51`, `#0c8ce9`). You can delete that block; the theme replaces it. Leaving
it in place is harmless (the theme loads later and wins).

## Step 3 — Switch to light mode in `master.blade.php`

Change the `<html>` class from `dark-style` to `light-style`:

```diff
- <html lang="en" dir="ltr" class="dark-style layout-navbar-fixed layout-menu-fixed sf-js-enabled"
+ <html lang="en" dir="ltr" class="light-style layout-navbar-fixed layout-menu-fixed sf-js-enabled"
```

A ready-made `master.blade.php` with this change is included in this folder.

---

## What you get
- Soft cool-gray background with subtle brand-blue glow (like the website)
- Floating glass sidebar + floating pill topbar
- Rounded floating cards with soft shadows
- Brand gradient (`#19AAFD → #006DF7`) on primary buttons, active menu items,
  tabs, pagination, badges
- Cleaner tables, forms, inputs, modals, tabs, breadcrumbs
- Plus Jakarta Sans for UI, JetBrains Mono for numbers/codes
- Fully responsive (the template's existing collapse/off-canvas still works)

No class names were changed, so direct-edit / future template updates keep working.

---

## Buttons audit (from your uploaded views)

**All the buttons you listed are already correctly wired** to Laravel routes —
none are fake static buttons. Verified in the views:

| Action | Route / mechanism | Status |
|---|---|---|
| Create New Task | `route('dashboard.tasks.create')` | ✅ wired |
| New Freelancer | `route('dashboard.freelancers.create')` | ✅ wired |
| New Client | `route('dashboard.clients.create')` | ✅ wired |
| View | `route('dashboard.*.show', $model)` | ✅ wired |
| Edit | `route('dashboard.*.edit', $model)` | ✅ wired |
| Delete | POST + `@method('DELETE')` to `*.destroy` | ✅ wired |
| Back | `route('dashboard.*.index')` | ✅ wired |
| Task Details / Files History tabs | Bootstrap `data-bs-toggle="tab"` | ✅ wired |
| Freelancer PO / Client PO | `tasks.vendor-pos.index` / `tasks.client-pos.index` | ✅ wired |
| Add File (modal) | axios POST `tasks.attachments.store` | ✅ wired |
| Download / Delete file | `tasks.attachments.download` / `.destroy` | ✅ wired |
| Logout | POST `dashboard.logout` | ✅ wired |

### ⚠️ The one thing I can't verify
You uploaded only the **Blade views** — not `routes/web.php` or the controllers.
So I can see every route a button *calls*, but I cannot confirm those routes are
actually **registered** on the backend. If a specific button does nothing in the
live app, the cause is a missing route/controller binding on the server, not the
markup. To verify or fix those, share:

- `routes/web.php` (the `dashboard.*` group)
- the relevant controllers (e.g. `TaskController`, `FinanceController`)

I did **not** add any fake buttons or guess at missing routes.
