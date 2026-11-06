# SecureNotes — Make It Fast & Fair, Then Ship It

**Weeks 16–17 Laboratory · work in pairs**

The SecureNotes API is already built, secure, and tested (Weeks 13–15). Your job
this lab is the last mile: make the front end **accessible** and **fast**
(Week 16), then wire up a **pipeline** that runs every check automatically
(Week 17), and finally **ship it** with a peer review (Part C).

Edit only inside the `>>> TASK n ... <<< END TASK n` markers. Work top to bottom.

---

## Setup

```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt

uvicorn app.main:app --reload   # open http://127.0.0.1:8000/app/
pytest -q                       # the app's 12 tests pass; many checks start red
```

The app itself is done: `test_api.py` (5) and `test_security.py` (7) are green and
you **don't touch them**. Most checks in `test_accessibility.py`,
`test_performance.py`, and `test_pipeline.py` start red — a few already hold.
Your job is to turn every red one green without breaking a green one.

---

## Part A — Make it accessible & fast (Week 16)

Files: `app/static/index.html` and `app/static/styles.css`
Check with: `pytest tests/test_accessibility.py tests/test_performance.py`

- [x] **TASK 1 — Perceivable.** Add `lang` to `<html>`, a real `<title>`, and
      `alt` text on both images.
- [x] **TASK 2 — Structure & controls.** Make the content a `<main>` landmark,
      fix the heading levels so they don't skip (h1 → h2, not h1 → h3), and turn
      the fake `<div class="btn">` into a real `<button>`.
- [x] **TASK 3 — Labels.** Replace the `<span class="fake-label">` text with real
      `<label for="...">` tied to each input's `id`.
- [x] **TASK 4 — Contrast.** In `styles.css`, darken `--ink` and `--brand` until
      both contrast tests pass (WCAG AA = 4.5:1).
- [x] **TASK 5 — Performance.** `defer` the script, add `width`/`height` to both
      images, and `loading="lazy"` to the content image.

## Part B — Ship it on a pipeline (Week 17)

File: `.github/workflows/ci.yml`
Check with: `pytest tests/test_pipeline.py`

- [x] **TASK 6 — Triggers.** Run the workflow on `push` and `pull_request` to `main`.
- [x] **TASK 7 — The job.** One `test` job on `ubuntu-latest` that checks out the
      code, sets up Python, installs `requirements.txt`, and runs `pytest`.

When all three suites pass locally, run the whole thing once more:

```bash
pytest -q        # everything green
```

## Part C — Push & peer review

- [ ] Push the repository to GitHub and publish the `fast-and-fair` branch.
- [ ] Open the pull request (`fast-and-fair` → `main`) and confirm GitHub Actions finishes with a green check.
- [ ] Review another pair's pull request with line comments, then submit an Approve or Request changes verdict.

These Part C steps need your GitHub account and a classmate's pull request, so they are left unchecked until completed.

```bash
git init
git add . && git commit -m "Starter: SecureNotes front end + empty pipeline"
git branch -M main
git remote add origin https://github.com/<you>/securenotes-shipit.git
git push -u origin main
git checkout -b fast-and-fair
# ...do Parts A and B...
git add . && git commit -m "Accessible + fast front end, CI pipeline green"
git push -u origin fast-and-fair
```

Open a pull request (`fast-and-fair → main`). On the **Actions** tab, watch your
pipeline run — it should end in a green check. Then swap PRs with another pair,
pull their branch, run `pytest`, and leave **line comments** on the diff before
you **Approve** or **Request changes**.

---

## Reflection

1. A real label connected to an input is easy for a sighted mouse user to overlook, but it helps screen-reader users identify each field and lets users click the label to focus its input. Making the login control a real button also lets keyboard users reach it and activate it with standard keyboard controls.
2. The CI pipeline runs the entire test suite on every push and pull request to `main`, so missing labels, contrast failures, performance regressions, and other tested problems are detected before a change is merged. This makes the checks repeatable and reduces the chance that someone accidentally ships a broken page weeks later.
