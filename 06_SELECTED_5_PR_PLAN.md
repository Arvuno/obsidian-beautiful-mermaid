# 06 SELECTED_5_PR_PLAN — obsidian-beautiful-mermaid

## Selected: Top 5 PRs

### PR 1: Add GitHub Actions CI for PRs
**Rationale**: Lowest effort, highest immediate value — ensures every PR is tested before merge  
**Branch**: `ci/pr-testing`  
**Changes**:
- Create `.github/workflows/ci.yml`
- Runs on: push to PR branches, merge to main
- Steps: setup node, install deps (`--legacy-peer-deps`), typecheck, build, upload artifact

---

### PR 2: Playwright End-to-End Tests  
**Rationale**: Most impactful long-term improvement — enables confident refactoring and catches regressions  
**Branch**: `test/playwright-e2e`  
**Changes**:
- Add `tests/` directory with Playwright setup
- Test: mermaid rendering produces SVG
- Test: settings changes persist
- Test: modal opens/closes
- Update package.json with `test` script

---

### PR 3: Add npm Vulnerability Fix Workflow  
**Rationale**: Addresses known vulnerability without breaking changes  
**Branch**: `workflow/audit-remediation`  
**Changes**:
- Create `.github/workflows/audit.yml`
- Weekly schedule + manual trigger
- Runs `npm audit --audit-level=moderate`
- Creates issue if vulnerabilities found

---

### PR 4: Improve TypeScript Strictness  
**Rationale**: Better type safety with minimal risk  
**Branch**: `chore/strict-types`  
**Changes**:
- Modify `tsconfig.json`: remove `skipLibCheck`, add `strictNullChecks`, `noImplicitAny`
- Fix any resulting type errors in `main.ts`
- Expected: minimal changes due to well-typed codebase

---

### PR 5: Add CodeCov / Coverage Reporting  
**Rationale**: Provides visibility into test coverage, enables coverage regression prevention  
**Branch**: `test/coverage`  
**Changes**:
- Add `c8` devDependency
- Update `package.json` scripts to collect coverage
- Add `.github/workflows/coverage.yml` 
- Add `codecov.yml` configuration
- Integrate with PR CI workflow

---

## Execution Order
1. CI workflow (PR 1) — validates all subsequent changes
2. TypeScript strictness (PR 4) — small, focused
3. Playwright tests (PR 2) — largest change, benefits from CI
4. Audit workflow (PR 3) — independent, low risk
5. Coverage (PR 5) — depends on tests existing

## Risks
- **PR 2**: Playwright may require Obsidian in testing context — use obsidian-testing-library approach
- **PR 4**: Removing `skipLibCheck` may reveal type issues in `beautiful-mermaid` types — may need `// @ts-ignore` locally