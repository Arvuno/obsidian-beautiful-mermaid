# 05 PR_CANDIDATES — obsidian-beautiful-mermaid

## 5 PR Candidates

### Candidate 1: Add Playwright End-to-End Tests
**Problem**: No test script defined in package.json, no automated tests exist  
**Impact**: High — prevents regression detection, blocks confident refactoring  
**Effort**: Medium  
**Files touched**: `package.json`, new `tests/` directory  
**Description**: Add Playwright tests that:
- Load the plugin in an Obsidian test environment
- Verify mermaid code blocks render to SVG
- Test settings changes propagate correctly
- Validate the modal zoom/pan interaction

---

### Candidate 2: Add npm Vulnerability Fix Workflow  
**Problem**: `npm audit` reports 1 moderate severity vulnerability; no automated remediation  
**Impact**: Medium — dependency vulnerabilities can compound  
**Effort**: Low  
**Files touched**: `.github/workflows/audit.yml` (new)  
**Description**: Add a weekly GitHub Actions workflow that runs `npm audit` and creates an issue/PR if vulnerabilities are found above a threshold.

---

### Candidate 3: Improve TypeScript Strictness  
**Problem**: `skipLibCheck` used, potentially hiding type issues in dependencies  
**Impact**: Medium  
**Effort**: Low-Medium  
**Files touched**: `tsconfig.json`, potentially `main.ts`  
**Description**: 
- Remove `skipLibCheck` and add proper type declarations
- Enable `strictNullChecks`, `noImplicitAny`, `exactOptionalPropertyTypes`
- Fix any resulting type errors

---

### Candidate 4: Add GitHub Actions CI for PRs  
**Problem**: No CI runs on PRs; build/s type-check only on release tags  
**Impact**: Medium — no automated verification of contributions  
**Effort**: Low  
**Files touched**: `.github/workflows/ci.yml` (new)  
**Description**: Add CI workflow that on PR:
- Runs `npm install --legacy-peer-deps`
- Runs `npm run typecheck`
- Runs `npm run build`
- Uploads `main.js` as artifact

---

### Candidate 5: Add CodeCov / Coverage Reporting  
**Problem**: No test coverage tracking  
**Impact**: Low-Medium  
**Effort**: Medium  
**Files touched**: `package.json`, `.github/workflows/coverage.yml` (new)  
**Description**: 
- Add c8 coverage to typecheck/build step
- Upload coverage to CodeCov on PR
- Set up coverage budget/baseline to prevent regression

---

## Summary Table

| # | Candidate | Impact | Effort | Priority |
|---|-----------|--------|--------|----------|
| 1 | Playwright E2E tests | High | Medium | 1 |
| 2 | npm audit workflow | Medium | Low | 3 |
| 3 | TypeScript strictness | Medium | Low-Medium | 4 |
| 4 | GitHub Actions CI | Medium | Low | 2 |
| 5 | Coverage reporting | Low-Medium | Medium | 5 |