# Dry-run Patch Plan — `chore/remove-brand-final`

Purpose: list all proposed, source-only replacements to remove the brand name "Kadena" and canonicalize Mission & Vision to Pact wording. This is a dry-run: no files will be changed. Review these replacements and mark any URL/repo-name changes as "risky" before applying.

Canonical replacements (case-insensitive):
- Org name: `Kadena Pact Community Foundation`  -> `Pact Community Organization`
- Short brand: `Kadena` -> `Pact` (use judgement in code/URLs)
- Domain: `kadena-community.org` -> `pact-community.org`
- Mission (exact): "Make it easy and safe for businesses to start building with Pact." (replace any mission line)
- Vision (exact): "A trusted Pact ecosystem where businesses can confidently start using audited, reliable open source Pact contracts." (replace any vision line)

High-level rules:
- Do change human-readable text in docs, wikis, READMEs, issue templates, CONTRIBUTING, ONBOARDING, and Pages `docs/` content.
- Do not change generated site bundles (`.next`, `out`, `website_clean`) here — instead rebuild site after source edits.
- Treat GitHub repo/org names and live URLs as "risky": list them for review and only change after confirmation.
- Avoid changing logs, migration outputs, or archived artifacts unless requested.

Files selected for patch proposals (source-only, grouped):

A. Website source (high priority)
- `tmp_website_repo/src/components/contracts/ContractCatalog.tsx`
  - Proposed safe change: Replace any user-visible text that says "Kadena" with "Pact".
  - Risky change: `https://github.com/kadena-community/sample-contract-1` -> `https://github.com/pact-community/sample-contract-1` (only if repo exists or you want links updated). Mark as REVIEW.

- `tmp_website_repo/docs/homepage-requirements.md`
  - Replace mission/vision paragraphs with canonical Pact mission & vision.
  - Replace `kadena-community.org` -> `pact-community.org` in domain references.

B. Mirrored website repo
- `kadena-community-website/src/components/contracts/ContractCatalog.tsx`
- `kadena-community-website/src/components/home/DocsHub.tsx`
- `kadena-community-website/src/components/docs/DocsHub.tsx`
- `kadena-community-website/src/app/globals.css`
- `kadena-community-website/docs/homepage-requirements.md`
  - For each: replace user-visible occurrences of "Kadena" → "Pact" and update domain mentions.
  - For repo/URL references to `kadena-community`, list as REVIEW (do not auto-change).

C. Repo & org docs
- `ONBOARDING.md`
  - Replace references to Kadena/Kadena Pact Community Foundation with Pact/Pact Community Organization.

- `./.github/PULL_REQUEST_TEMPLATE.md`
  - Replace any checklist or text referencing Kadena to Pact.

D. Contributing & governance
- `conflict_diffs/foundation/chore_point-docs-to-pact-5__CONTRIBUTING.md.diff`
  - Apply same text replacements in the patch artifacts (these are review artifacts; keep in diffs consistent with source changes).

E. Other docs and artifacts (low risk; non-production)
- `issue5.json`, `migration_search_results.txt`, `migration_website_search.txt`, `pages_before.json`, `pages_after.json` — These are exports/migration artifacts. Do NOT modify unless you want historic records updated. List them here so maintainers can decide.

F. Scripts & helpers
- `scripts/generate_summary.sh` — update the user-facing echo lines and any canonical strings printed; safe to change.

G. Summary file
- `foundation/pact-changes-summary.md` — already generated; review and update language to match final changes.

Per-file proposed snippets (examples)

- `tmp_website_repo/docs/homepage-requirements.md`
  - Original example line: `Make it easy and safe for businesses to start building on Kadena.`
  - Proposed: `Make it easy and safe for businesses to start building with Pact.`

  - Original: `A trusted Kadena ecosystem where businesses can confidently start using audited, reliable open source Pact contracts.`
  - Proposed: `A trusted Pact ecosystem where businesses can confidently start using audited, reliable open source Pact contracts.`

- `ONBOARDING.md`
  - Original: `The Kadena Pact Community Foundation is ...`  
  - Proposed: `The Pact Community Organization is ...`

- `./.github/PULL_REQUEST_TEMPLATE.md`
  - Replace: `kadena-community.org` -> `pact-community.org` (if present)

Risky changes (require review):
- Any `https://github.com/Kadena-Pact-Community-Foundation/...` or `https://github.com/kadena-community/...` links: confirm new org/repo names before changing.
- Any package names in `package.json` or `package-lock.json` referencing `kadena-community` may break package imports; list them but treat as REVIEW.

Files found by the dry-run search (source-only) — candidate set (trimmed):
- `tmp_website_repo/src/components/contracts/ContractCatalog.tsx`
- `tmp_website_repo/src/app/globals.css`
- `tmp_website_repo/docs/homepage-requirements.md`
- `kadena-community-website/src/components/contracts/ContractCatalog.tsx`
- `kadena-community-website/src/components/home/DocsHub.tsx`
- `kadena-community-website/src/components/docs/DocsHub.tsx`
- `kadena-community-website/src/app/globals.css`
- `kadena-community-website/docs/homepage-requirements.md`
- `ONBOARDING.md`
- `.github/PULL_REQUEST_TEMPLATE.md`
- `scripts/generate_summary.sh`
- `foundation/pact-changes-summary.md`
- `conflict_diffs/foundation/chore_point-docs-to-pact-5__CONTRIBUTING.md.diff`

(There are additional logs and migration artifacts found — these are listed in the grep output. I did not include logs in the authoritative-change set.)

Command set to produce a *dry-run patch* locally (shows unified diffs without changing files):

```bash
# generate a list of candidate files (already done earlier)
# produce a per-file preview -- show lines with 'kadena' and a sed replacement preview
for f in $(cat /tmp/kadena_files.txt); do
  echo "--- $f ---"
  grep -In --color=always "kadena" "$f" || true
  echo "Preview replacement (sed output) -- first 20 lines:"
  sed -n '1,200p' "$f" | sed -e 's/[Kk]adena/Pact/g' | sed -n '1,20p'
  echo
done
```

Exact patch application (example, DO NOT RUN until reviewed):

```bash
# create a review branch
git checkout -b chore/remove-brand-final

# apply replacement safely for text files only (preview first)
git ls-files | grep -E '\.(md|txt|json|yml|yaml|ts|tsx|css)$' | xargs -I{} bash -c "
  if grep -qi 'kadena' '{}'; then
    echo 'Updating {}'
    sed -E -i.bak -e 's/Kadena Pact Community Foundation/Pact Community Organization/g' \
                 -e 's/[Kk]adena\-community\.org/pact-community.org/g' \
                 -e 's/[Kk]adena/Pact/g' '{}' ;
    git add '{}';
  fi
"

# commit
git commit -m "chore: replace Kadena references with Pact; canonical mission & vision (dry-run preview applied)"
```

Notes and guidance for maintainers:
- Review the dry-run plan and flag any URLs/repo names that should not be changed automatically.
- After approving the dry-run, I can apply the changes, commit to `chore/remove-brand-final`, and prepare a PR draft with `pact-changes-summary.md` and a clear list of changed files.
- After merge, rebuild the site to regenerate `out/` and `.next/` so the public site no longer contains legacy references.

Next steps (I can take now):
1. Produce a more detailed per-file unified-diff preview for the prioritized authoritative files (safe changes only).  
2. After your review, apply changes and commit to a branch (I will not push or open PRs without your confirmation). 

Which next step do you want: (1) detailed per-file diff previews, (2) apply safe changes and create a local commit, or (3) adjust rules and re-run the dry-run with different filters?