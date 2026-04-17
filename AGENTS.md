# AGENTS.md

> Enterprise operating manual for autonomous or assisted contributors working on the **AMANI Documentation Platform**.

---

## 1. Mission

Maintain a reliable, accurate, version-controlled documentation system for the AMANI ecosystem. Contributors must prioritize:

1. Accuracy
2. Reproducibility
3. Minimal-risk changes
4. Clear information architecture
5. Fast recovery from regressions
6. Consistent developer experience

---

## 2. Technical Baseline

### Core Stack

- **Documentation Engine:** MkDocs
- **Theme:** Material for MkDocs (if configured)
- **Package / Environment Manager:** **uv (Astral)**
- **Language:** Markdown + YAML
- **Optional Assets:** HTML, CSS, JS, generated docs

### Package Policy

Use `uv` as the default package/runtime tool.

**Approved:**

```bash
uv venv
uv sync
uv run mkdocs serve
uv run mkdocs build --strict
```

**Disallowed unless explicitly requested:**

```bash
pip install ...
pipenv ...
poetry install
```

---

## 3. Repository Operating Model

Typical structure:

```text
docs/             Primary content
api/              API references
arquitectura/     Architecture and diagrams
guias/            Operational guides
javadoc/          Generated Java docs
kdoc/             Generated Kotlin docs
_site/            Generated static output
mkdocs.yml        Site configuration
```

If actual structure differs, inspect repository state first and adapt conservatively.

---

## 4. Contributor Roles

### Documentation Agent
Creates or updates Markdown content.

### Build Agent
Validates MkDocs build integrity.

### Refactor Agent
Improves structure/navigation with backward compatibility.

### Audit Agent
Finds stale pages, broken links, duplication, weak ownership.

No agent should perform destructive changes without justification.

---

## 5. Non-Negotiable Rules

### Never Edit Generated Output Manually

Do not manually edit:

- `_site/`
- `search/`
- `sitemap.xml`
- `sitemap.xml.gz`
- generated `javadoc/`
- generated `kdoc/`

### Preserve Stability

Do not rename/move pages casually. Existing URLs may be referenced externally.

### Change Minimum Necessary Surface Area

Prefer surgical edits over sweeping rewrites.

### Validate Before Completion

Every meaningful change should pass:

```bash
uv run mkdocs build --strict
```

---

## 6. Standard Workflows

## A. Add New Documentation

1. Inspect `mkdocs.yml`
2. Identify correct domain section
3. Create concise Markdown page
4. Add navigation entry if needed
5. Cross-link related pages
6. Run strict build
7. Report results

## B. Fix Broken Build

1. Reproduce error locally
2. Isolate failing file/config
3. Apply smallest viable fix
4. Rebuild strictly
5. Document root cause

## C. Restructure Navigation

1. Map current nav tree
2. Identify duplication/confusion
3. Propose low-risk improvement
4. Preserve old paths where possible
5. Validate discoverability

## D. Large Content Migration

1. Inventory pages
2. Detect duplicates
3. Merge carefully
4. Add redirects if supported
5. Re-run build/tests

---

## 7. Documentation Standards

### Writing Style

- Direct
- Technical
- Low ambiguity
- Actionable
- Concise

### Markdown Rules

- One H1 per page
- Logical heading hierarchy
- Fenced code blocks with language tags
- Relative links preferred unless external
- Tables only when useful

### Example Code

Use runnable or realistic examples. Avoid pseudo-code unless labeled.

---

## 8. Quality Gates

## Required Gate

```bash
uv run mkdocs build --strict
```

## Recommended Gate

Check manually:

- navigation order
- internal links
- mobile readability
- code formatting
- duplicate sections
- outdated screenshots
- spelling of technical terms

---

## 9. Security & Compliance

Never publish:

- secrets
n- API keys
- tokens
- credentials
- internal IPs (unless intended)
- private customer data
- sensitive logs

Sanitize examples before commit.

---

## 10. Decision Framework

When uncertain, prefer:

1. Reversible change
2. Smaller diff
3. Backward compatibility
4. Explicit note over hidden assumption
5. Existing conventions over invention

---

## 11. PR / Change Report Format

```text
Summary:
- What changed

Files:
- path/file.md
- mkdocs.yml

Validation:
- uv run mkdocs build --strict ✅

Risk:
- Low / Medium / High

Follow-up:
- Optional next improvements
```

---

## 12. Escalation Conditions

Pause autonomous modification and request owner review when:

- deleting many files
- changing global navigation heavily
- replacing generated docs pipeline
- uncertain legal/compliance content
- contradictory source material
- hidden build failures persist

---

## 13. Anti-Patterns

Do not:

- mix `pip` workflows into uv-managed repo
- duplicate docs instead of improving originals
- silently remove pages
- add dependencies casually
- rewrite tone inconsistently across site
- ignore warnings because build passes

---

## 14. Fast Commands

```bash
# setup
uv venv
uv sync

# dev server
uv run mkdocs serve

# production validation
uv run mkdocs build --strict
```

---

## 15. Agent Success Metric

A successful contribution means:

- clearer docs
- zero regressions
- validated build
- easy rollback
- better developer usability

---

## 16. Default Operating Assumption

If context is incomplete: inspect first, change minimally, validate fully, report clearly.

