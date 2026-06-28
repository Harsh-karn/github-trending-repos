# Dependency Audit — vitalets/github-trending-repos

## Summary
- **11 vulnerabilities found**: 1 low, 4 moderate, 6 high
- Several packages have major version upgrades available

---

## High Severity (fix these first)

### `axios` — `^0.20.0` → `1.18.1` (breaking)
- 20+ CVEs including SSRF, CSRF, prototype pollution, ReDoS, header injection
- **Fix**: `npm install axios@1.18.1`
- ⚠️ Breaking change — check all `axios` call sites in `scripts/`

### `ip` — `^1.1.5` → `2.0.1`
- SSRF via improper `isPublic()` categorization (GHSA-2p57-rm9w-gvfp)
- **Fix**: `npm install ip@2.0.1`

### `parse-link-header` — `^1.0.1` → `2.0.0` (breaking)
- Uncontrolled resource consumption (GHSA-q674-xm3x-2926)
- **Fix**: `npm install parse-link-header@2.0.0`
- ⚠️ Breaking change — API may differ, test usage in scripts

### `minimatch` (via `mocha`) — inherited vuln
- Multiple ReDoS CVEs
- **Fix**: `npm audit fix` (non-breaking, transitive)

### `serialize-javascript` (via `mocha`) — inherited vuln
- RCE and DoS via crafted inputs
- **Fix**: `npm audit fix` (non-breaking, transitive)

---

## Moderate Severity

### `js-yaml` (via `eslint` and `mocha`) — prototype pollution
- **Fix**: Upgrade eslint to v10 — `npm install eslint@10.6.0` (breaking)

### `nanoid` (via `mocha`) — predictable output
- **Fix**: `npm audit fix`

---

## No-breaking Outdated Packages (safe to bump)

| Package | Current | Latest | Notes |
|---|---|---|---|
| `axios-retry` | `^3.1.8` | `4.5.0` | Major bump, check API |
| `emoji-regex` | `^9.0.0` | `10.6.0` | Major bump |
| `fs-extra` | `^9.0.1` | `11.3.5` | Major bump |
| `ramda` | `^0.27.1` | `0.32.0` | Minor API changes |
| `cheerio` | `^1.0.0-rc.2` | `1.2.0` | Stable release now! rc → stable |
| `@vitalets/google-translate-api` | `^9.0.0-1` | `9.2.1` | Safe minor bump |

---

## Recommended PR Approach

**PR 1 (low risk — `npm audit fix`):**
Run `npm audit fix` to fix transitive deps in mocha (minimatch, nanoid, serialize-javascript, diff).
No code changes needed.

**PR 2 (medium risk):**
- Bump `ip` to `2.0.1`
- Bump `cheerio` from rc to stable `1.2.0`
- Bump `ramda`, `fs-extra`, `axios-retry`, `emoji-regex`

**PR 3 (high risk — needs testing):**
- Upgrade `axios` to `1.18.1` — audit all usages in scripts/
- Upgrade `parse-link-header` to `2.0.0`
- Upgrade `eslint` to `10.x`

---

## Quick commands to run locally after forking

```bash
# Safe fixes (no breaking changes)
npm audit fix

# Check what's still broken
npm audit

# Bump specific packages manually
npm install ip@2.0.1 cheerio@1.2.0 ramda@0.32.0 fs-extra@11.3.5

# Run tests to verify nothing broke
npm run test-helpers
```
