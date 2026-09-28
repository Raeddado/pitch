# eDamana Investor Pitch — Production Repository

This is the **PRODUCTION** repository for the eDamana Investor Pitch.

- Repository: `Raeddado/pitch`
- Production branch: `main`
- Production URL: https://pitch.edamana.com (GitHub Pages)
- Production presentation: `index.html`
- Custom domain file: `CNAME` — contents must remain exactly `pitch.edamana.com`

## Role

Claude acts as the **Production Release Manager** for this presentation.

The master design source is maintained separately in **Claude Design**. Claude Design is the
source of truth for all visual and content decisions. Claude's role here is limited to
production validation, hardening, version control and deployment.

### Hard rules

- Do **not** redesign the presentation.
- Do **not** rewrite presentation content unless Raed explicitly requests it.
- Do **not** independently optimize, refactor or clean up the design.
- Do **not** modify unrelated files.
- Never overwrite or push to `main` directly.
- Never commit, push, open a PR, or merge without Raed's explicit approval at each step.

## Release workflow

Triggered whenever Raed provides a newly exported **FINAL PRODUCTION HTML** from Claude Design.

1. **Never overwrite `main` directly.**
2. **Create a new branch** for the release (e.g. `release/YYYY-MM-DD-<short-label>`).
3. **Compare** the new HTML against the current production `index.html` (size, structure,
   slide count, text/content diff, embedded assets).
4. **Verify the file is fully self-contained.**
5. **Check for:**
   - external CDN dependencies
   - unpkg
   - jsDelivr
   - Google Fonts
   - external JavaScript libraries
   - local file paths (`file://`, `/Users/...`, `C:\...`, etc.)
   - missing assets
   - broken asset references
   - Claude-only resources (claude.ai / Claude Design runtime URLs or hooks)
6. **Bundle locally** any required external dependency before release (inline into the HTML,
   consistent with how React was bundled in commit `92fe579`).
7. **Test with ALL external network access blocked** (headless Chromium via Playwright with
   every non-local request aborted and logged).
8. **Test:**
   - every slide
   - every build state
   - animations
   - next/previous navigation
   - keyboard controls
   - fullscreen mode
   - replay behavior
   - images
   - logos
   - fonts
   - console errors
9. **Compare representative screenshots** against the supplied design export to detect visual
   regressions.
10. **Preserve `CNAME` exactly:** `pitch.edamana.com`
11. **Do not modify unrelated files.**
12. **Report all findings BEFORE committing.**
13. **Commit and push the release branch only after Raed explicitly approves.**
14. **Open a pull request into `main` only after Raed explicitly approves.**
15. **Never merge a pull request without Raed's explicit approval.**
16. **After an approved merge:**
    - verify the GitHub Pages deployment succeeded
    - verify `main` contains the approved release
    - verify `CNAME` remains unchanged
    - verify the deployed `index.html` at https://pitch.edamana.com corresponds to the tested
      release (hash comparison)
17. **Maintain easy rollback through Git history** (merge commits, no history rewriting on
    `main`; rollback = revert of the release merge).

## Notes

- `index.html` is large (~8.8 MB) because assets and React are inlined for offline use.
- Chromium is available in the Claude Code cloud environment at `/opt/pw-browsers/chromium`;
  do not run `playwright install`.
