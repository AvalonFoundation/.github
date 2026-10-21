# Rules for coding agents

The org's public `.github` repo: the profile README (`profile/README.md`) and the default issue forms (`.github/ISSUE_TEMPLATE/`). GitHub uses these forms in every AvalonFoundation repo, private ones included, that has no issue templates of its own. A repo with its own `ISSUE_TEMPLATE` folder (design-ui-kit) gets none of them, so it carries copies. Conventions and decisions live in the [handbook](https://github.com/AvalonFoundation/handbook) (`~/Source/handbook` when cloned).

## Rules

- This repo is public. Nothing private goes in it: no unreleased plans, names of private repos' internals, or tokens.
- When a form changes here, change design-ui-kit's copy in the same session.
- The agent task form's Verification field is required: an issue without a verification command can't be `ready`.
- The forms set an issue type (Task, Epic, Bug). Those types are the org's; only an owner adds one.

## Commands

- Check the forms parse: `python3 -c "import yaml,glob; [yaml.safe_load(open(f)) for f in glob.glob('.github/ISSUE_TEMPLATE/*.yml')]"`
- Preview them: open https://github.com/AvalonFoundation/lib-ui/issues/new/choose after merging.

## Depends on

No `lib-` crates.

## Work queue

Work comes from GitHub issues; they are the only record of what's in progress. The full process is the handbook's [agent workflow](https://github.com/AvalonFoundation/handbook/blob/main/docs/process/agent-workflow.md), and the `avalon` plugin's skills (`work-issue`, `decompose-epic`, `handoff`, `dispatch`) run it. In short:

- **Workable:** open, labelled `ready`, not `in-progress` or `needs-human`, and no open `blocked-by` issues. Only one issue is in progress across the org at a time.
- **Claim:** add `in-progress` and comment `Claimed by session <id> at <UTC time>`. Re-read the issue; if an earlier claim that isn't stale (no activity for 24 hours) exists, remove your label and back off.
- **Build:** branch `issue-<n>-<slug>`, never push to `main`. Do only the acceptance criteria. Anything else you find becomes a new issue labelled `proposed`; never label an issue `ready`.
- **Verify:** run the issue's verification command and this repo's checks (below).
- **PR:** the body says `Closes AvalonFoundation/<repo>#<n>` and how it was verified. Squash-merge once CI is green, unless the issue asks the owner to review a comparison first: then label the PR `needs-human` and leave it.
- **Stop for the owner:** for anything architectural, licensing-related or hard to reverse, comment on the issue, label it `needs-human`, remove `in-progress`, and move on.
- **Hand off:** whenever a session stops, finished or not, comment what's done, what's next, gotchas, the branch or PR, and the state of verification.
- Never edit decision records or the roadmap, except in a PR labelled `needs-human`. No progress or status files: state lives in issues.

## This repo's checks

The forms parse (above). There's no CI.
