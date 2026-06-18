---
name: x-to-obsidian
description: Use when Codex needs to batch collect high-view X/Twitter posts from creator/account lists and save them into Obsidian with the official Obsidian Web Clipper. Trigger for requests like “抓取/扒取 X 推文到 Obsidian”, “按浏览量筛选博主推文”, “用 Obsidian Web Clipper 保存推文/长文”, or “把对标博主爆款推文归档”. This skill is for macOS Chrome/Dia workflows where X is logged in and Obsidian Web Clipper is installed.
---

# X To Obsidian

## Core Rule

Use the bundled script as the execution path. Do not generate final tweet Markdown yourself: Obsidian Web Clipper must extract and create the initial note body. The script may only find candidates, open the correct page, trigger Web Clipper, move the created note, add metadata, dedupe, and report.

The default deliverable is always notes written into the active Obsidian vault. Keep user-facing setup and confirmation text short. Do not explain CSV/JSON/Markdown export boundaries unless the user asks about exports or the environment cannot save to Obsidian. If the environment cannot save through Obsidian Web Clipper, stop and explain the missing setup.

Default to fewer questions. If the user gives an account URL plus an explicit count, proceed with the default browser, threshold, days, clip mode, and close-after-save behavior. If the user gives an account URL without a count, ask only for the count.

Never relax the view threshold on behalf of the user. If the default `views > 90000` search does not find enough candidates, broaden only the search depth/time window and keep the same threshold. Save fewer posts or stop with a clear "not enough high-view candidates found" message rather than saving low-view posts.

## Workflow

1. Treat requests like "帮我扒取 https://x.com/..." as Obsidian-save requests by default. Do not ask whether the target is Obsidian unless the user mentions another destination or export-only output.
2. Use defaults to reduce user input:
   - Browser: Dia.
   - Profile: Dia `Profile 1`, Chrome `Default`.
   - View threshold: `90000`.
   - Initial time range: `90` days.
   - If fewer high-view candidates than requested are found, auto-expand to `365` days and deeper pagination while keeping `views > 90000`.
   - Clip mode: `popup`.
   - Close target tab after successful save: enabled.
3. Before collecting optional inputs, make sure the setup is ready:
   - If the user has not installed the official Obsidian Web Clipper, run `--setup` or `--open-web-clipper-install`.
   - The script may open the Chrome Web Store page, but the user must manually click install and confirm permissions.
   - After the user says installation is done, run `--preflight`.
4. Ask only for missing information that is required to run:
   - If no account URL/list is provided, ask for the X account URLs.
   - If no count is provided for a single account, ask only "要保存几篇？" and mention the defaults in one short sentence.
   - If multiple accounts are provided and no count scope is clear, ask whether the number is total posts or per-account posts.
   - Do not ask the user to reconfirm browser, threshold, days, or Obsidian target when defaults are acceptable.
5. Parse the pasted account list into accounts with `display_name`, `handle`, and `url`.
6. Run a dry-run first only as candidate preview. Show candidate counts and examples; do not write to Obsidian yet, do not call it complete, and do not attach CSV/JSON/Markdown exports as the deliverable.
7. If the user already gave an imperative save request with count, proceed to save after dry-run without asking another confirmation. Otherwise ask a short confirmation.
8. Save with Web Clipper only:
   - Use X login cookies only to resolve accounts and filter by views/date.
   - Sort candidates by `views` descending before saving, so "save 3" means the highest-view eligible posts found, not the first 3 visible timeline posts.
   - Do not rerun with `--threshold 0`, unlimited views, or any lower threshold unless the user explicitly asks for that lower threshold. The script will reject low-threshold save mode unless `--allow-low-view-save` is deliberately passed.
   - Open tweet detail pages for normal posts.
   - Open `x.com/i/article/...` pages when the tweet points to an X Article, so Web Clipper captures the article body instead of only a `t.co` link.
   - Trigger Web Clipper by shortcut, then locate the new note in the currently open Obsidian vault.
   - Reject and quarantine a clip if the Web Clipper `source` URL does not match the target tweet/article URL. For X Articles, accept both `x.com/i/article/...` and canonical `x.com/{handle}/article/...` source URLs for the same article/status.
   - Move the note to the creator folder, prune known X page chrome/noise sections and same-thread prelude from normal tweet clips, and add metadata without synthesizing the clipped body.
   - Close only the script-opened target X tab after the note is verified and postprocessed. Leave the tab open when save/postprocess fails.
9. Report saved, skipped, failed, and already-processed counts.

## Preconditions

Require these before save mode:

- macOS.
- Chrome or Dia, with the target X account logged in.
- Official Obsidian Web Clipper installed in the selected browser profile.
- Obsidian open with the target vault active.
- System Events automation/Accessibility permission granted to the app running Codex.
- Web Clipper shortcut configured:
  - Default path: Open Clipper with `Cmd+Shift+O`, then Enter
  - Optional path: Quick Clip with `Alt+Shift+O` only when the user has verified it is stable in their browser

If any required condition fails, stop and explain the exact missing setup. Do not fall back to hand-built Markdown, CSV/JSON export, Markdown indexes, or DOM guessing.

If a run cannot write to Obsidian, the final response must say it is blocked and list the setup step needed (`--setup`, `--preflight`, Web Clipper install, Obsidian vault, X login, or macOS permissions). Do not say "扒好了" for a dry-run or export-only result.

## Script

Script path:

```bash
~/.codex/skills/x-to-obsidian/scripts/x_to_obsidian.py
```

First-time setup helper:

```bash
python3 ~/.codex/skills/x-to-obsidian/scripts/x_to_obsidian.py \
  --browser dia \
  --setup
```

Open only the official Web Clipper install page:

```bash
python3 ~/.codex/skills/x-to-obsidian/scripts/x_to_obsidian.py \
  --browser dia \
  --open-web-clipper-install
```

Default dry-run:

```bash
python3 ~/.codex/skills/x-to-obsidian/scripts/x_to_obsidian.py \
  --browser dia \
  --accounts-text "$ACCOUNTS_TEXT" \
  --threshold 90000 \
  --days 90 \
  --auto-expand-days 365 \
  --auto-expand-max-pages 80
```

Save after confirmation:

```bash
python3 ~/.codex/skills/x-to-obsidian/scripts/x_to_obsidian.py \
  --browser dia \
  --accounts-text "$ACCOUNTS_TEXT" \
  --threshold 90000 \
  --days 90 \
  --auto-expand-days 365 \
  --auto-expand-max-pages 80 \
  --save-required \
  --save \
  --clip-mode popup \
  --close-after-save
```

Useful options:

- `--browser chrome|dia`
- `--profile "Default"` for Chrome or `--profile "Profile 1"` for Dia
- `--accounts-file path/to/accounts.csv`
- `--accounts-text "...pasted account list..."`
- `--threshold 90000`
- `--allow-low-view-save` only when the user explicitly asks for a lower threshold; never use it as an automatic fallback
- `--days 90`
- `--auto-expand-days 365` to broaden the time window only when not enough candidates are found
- `--auto-expand-max-pages 80` to deepen pagination during the auto-expand retry
- `--save-required` to fail fast if an Obsidian-mode command accidentally omits `--save`
- `--max-pages 25`
- `--limit-saves 1` for testing
- `--clip-mode quick|popup|auto` where `popup` is the default for Dia/Chrome reliability
- `--close-after-save` to close the verified script-opened X tab after saving; default enabled
- `--no-close-after-save` to keep target tabs open while debugging
- `--preflight` to validate environment before saving
- `--setup` to guide first-time environment setup without fetching X or writing Obsidian
- `--open-web-clipper-install` to open the official Obsidian Web Clipper install page and exit

## Account Input

Accept pasted lists such as:

```text
AI产品黄叔
https://x.com/PMbackttfuture

数字生命卡兹克
https://x.com/Khazix0918
```

For ambiguous names, prefer the display name immediately above the URL. Sanitize folder names by replacing path separators and illegal filename characters with `-`.

## Acceptance Checks

Before calling the task complete, verify:

- Save tasks created at least one expected `.md` note inside the active Obsidian vault, unless all requested candidates were already processed.
- Dry-run found candidates and did not write files.
- Candidate selection preserved the requested/default view threshold; no low-view fallback was used.
- When more eligible candidates exist than requested, the saved posts are the highest-view candidates found.
- Save mode created notes through Web Clipper, not script-generated Markdown.
- X Article candidates save the article page body, not only the tweet URL or `t.co` link.
- Normal tweet clips do not keep X reply lists, "发现更多", "当前趋势", or "有什么新鲜事" sections.
- Normal tweet clips start at the target tweet text; quoted tweet cards may remain when they are part of the target tweet.
- Clips whose frontmatter `source` points to a different X URL are quarantined and reported as failed, not recorded as processed. X Article canonical URL variants are accepted.
- Notes are moved into `博主显示名/`.
- Metadata includes at least `tweet_id`, `views`, `original_tweet_url`, `clipped_url`, `author`, and `handle`.
- Successful saves close the script-opened target X tab; failed saves keep the page open for debugging.
- Re-running skips already processed tweet IDs.

## Response Rules

- For successful Obsidian saves, report the saved note paths, saved count, failed count, skipped/already-processed count, and any remaining blocker. Do not make CSV/JSON/report files the primary result.
- For dry-runs with a user-provided save count, treat the dry-run as an internal candidate check and continue into save mode. Do not ask another confirmation.
- For dry-runs without a user-provided save count, say only that candidates were found and ask for the missing count or save confirmation. Do not say the posts were imported or completed.
- If not enough candidates meet the threshold, say how many eligible candidates were found and ask whether to lower the threshold or expand beyond the configured search window. Do not save low-view posts automatically.
- For export-only requests, clearly state that the result is export-only and not imported into Obsidian.
- If setup is missing, stop at setup/preflight guidance and do not generate substitute exports.

## Boundaries

- Do not bypass X access controls, scrape private accounts, or attempt to evade rate limits.
- Do not support full automation outside macOS in v1.
- Do not promise Edge/Arc support unless their profile paths and Web Clipper shortcuts are explicitly verified.
- Do not use Obsidian URI as the default content creation path; it is only a diagnostic fallback if the user explicitly accepts non-Clipper output.
