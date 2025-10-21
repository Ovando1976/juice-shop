# Week-5 Lab Report — Cache, Auth Mapping, Automation (Juice Shop)

**Tester:** Ovando Rawlins  
**Date:** YYYY-MM-DD  
**Scope:** This sandbox only (localhost:3000 / \*.csb.app)

## 1) Cache & CDN Audit

Command: `npm run lab:cache > reports/cache.tsv`  
Attach/Link: `reports/cache.tsv`

**Observations**

- Public pages caching: _(public / max-age / etag / last-modified?)_
- Sensitive routes (login/account): _(no-store / private?)_
- Vary headers (privacy implications): _(...)?_

**Recommendations**

- Sensitive views: `Cache-Control: no-store`
- Static views: `public, max-age=..., immutable` + ETag
- Validate `Vary` usage (avoid leaking auth variants)

---

## 2) Cache Validators Drill (GET-only)

Command: `npm run lab:cache:drill`  
**Result summary**

- Baseline: `status=___ bytes=___ ETag="___" Last-Modified="___"`
- With validators: `status=___` (expect `304` for cacheable resources)

---

## 3) Auth Mapping (pre vs post)

Commands:

- Pre: `npm run lab:auth:pre  > reports/auth_pre.tsv`
- Post: `COOKIE='…' npm run lab:auth:post > reports/auth_post.tsv`

**Findings (summary)**

- Status differences: _…_
- Set-Cookie behavior: _…_
- Client storage (keys only): _…_

**Recommendations**

- Sessions in **HttpOnly** cookies; `Secure` (HTTPS), `SameSite=Lax/Strict`
- Short TTL + rotation; CSP to reduce XSS impact

---

## 4) Headers Baseline / Diff

Commands:

- `npm run lab:headers:diff` (console)
- `npm run lab:audit` → `reports/headers.csv` + `reports/findings.md`

**Summary**

- Present on all routes: _XFO, XCTO_
- Missing: _CSP, Referrer-Policy, Permissions-Policy_ (HSTS on HTTPS only)
- See: `reports/findings.md` and `reports/headers.csv`

---

## 5) Route Map & Budget

Commands:

- `npm run lab:routes` → `reports/routes.json`, `reports/routes.md`
- `LAB_ROUTE_BUDGET=120000 npm run lab:route:budget`

**Notes**

- Heavy pages ≥ 120 KB (HTML only): _list if any_
- See dashboard (`npm run lab:dash` on port 4500) for quick visualization.

---

## 6) Evidence Bundle

Command: `npm run lab:pack`  
Artifacts: `reports/evidence.zip` (or `.tar.gz`)

---

## 7) Executive Summary (one paragraph)

- Highest-value improvements (CSP, cookie flags, cache controls).
- CI guards in place (route/headers regression, weight budget).
