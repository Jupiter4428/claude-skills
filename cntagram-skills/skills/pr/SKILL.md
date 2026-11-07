---
name: pr
description: "Use when writing a PR body, drafting a review of someone's PR, or answering PR review comments."
metadata:
  credits:
    skill: show-me
    author: Dex Horthy
    organisation: Humanlayer
    url: "https://github.com/humanlayer/skills/blob/main/plugins/show-me/skills/show-me/SKILL.md"
---

Write every PR body in the team format below, in Thai, with code identifiers left as-is. Reference PRs that show it done: #41 (US-7) and #39 (US-4) (`gh pr view <n>`).

```markdown
Refs #<issue>

> PR นี้เป็นฝั่ง Backend ส่วน Frontend (@<owner>) จะเปิด PR แยกจาก `develop` หลัง PR นี้ merge แล้ว และใส่ `Closes #<issue>`

## Summary of Changes (Backend)
<one sentence: what the user can now do>
- `forms.py`: <each form, what it validates, the Thai error it shows>
- `services.py` / `selectors.py`: <each function and the rule or query trick it carries (N+1, index)>
- `views.py` + `urls.py`:
  - `<view>`: <what the page does, permission, method>
- <any other file touched, and why>

## AC (ส่วน Backend)
- [x] <AC copied verbatim from the issue>
- [ ] <AC not met yet> (<why, and who or what closes it>)

## เอกสาร
- <each doc updated, and each doc checked that needed no change>

## ข้อควรรู้
- <deviations from the issue wording, known gaps, TODOs left for another story>

## วิธีทดสอบ
`python manage.py test <app>` <N> tests OK

## Checklist (Definition of Done)
<CONTRIBUTING's DoD list, copied as-is>
```

## Rules

- **Issue link**: `Refs #N` while the story still has work in another PR (usually the Frontend one); `Closes #N` goes on the PR that finishes the story. Drop the blockquote note when one PR covers the whole story, and title the sections without "(Backend)".
- **Issue checklist**: a split story's issue carries one line per side, as CONTRIBUTING ("Story ที่มีทั้งงาน Backend และ Frontend") asks: `- [ ] Backend #<PR>` / `- [ ] Frontend #<PR>`, or `(ยังไม่เปิด PR)` for a side with no PR yet. Opening a PR adds its line and a merge ticks it. Show the user the issue edit and apply it once they agree.
- **Headings**: `## Summary of Changes`, `## วิธีทดสอบ` and `## Checklist (Definition of Done)` are the ones CONTRIBUTING ("Pull Request และ Code Review") requires; reviewers check for them.
- **Summary of Changes**: one bullet per file or layer, saying what it holds and the decision behind it. Name real functions, forms and URLs.
- **AC**: copy the issue's Acceptance Criteria word for word; tick only the ones a test or traced code path proves.
- **ข้อควรรู้**: every place the code departs from the issue, every known risk left unfixed, and every TODO, so the reviewer reads them before the diff.
- **วิธีทดสอบ**: the exact command and the real count from the last run.
- **Checklist (Definition of Done)**: tick only what you checked; an unchecked line says why in brackets.
- Write the body to a file in the scratchpad and pass it with `gh pr create/edit --body-file`.

## Drafting a review

First read the PR's earlier reviews (`gh pr view <n> --json reviews`). Each finding carries an earlier round's call forward: one already marked "ไม่ต้องแก้ใน PR นี้" stays there, and reversing a call means saying so in the finding, with the reason.

Turn `code-review` findings into one review comment, in Thai, shaped like the reviews on #49 and #56 (`gh pr view <n> --json reviews`):

```markdown
<one or two lines: what already works, and whether it can merge>

> เลขบรรทัดอ้างอิง commit `<head sha>`
> <test count on the branch, and on the tree merged with `develop`>
> โค้ดในหัวข้อ **แก้:** เป็นคำแนะนำในการแก้ ถ้ามีวิธีอื่นที่ได้ผลเดียวกันก็ใช้ได้เลยครับ

## ต้องแก้ก่อน merge
### 1. `<file>` L<start>–<end>: <what is wrong, in a few words>
<the current code, quoted>
<why it matters: the rule broken or the input that breaks it>

**แก้:**
<the fix as code>

## ควรแก้
### `<file>` L<n>: <title>   (same shape, unnumbered)
## Test ที่ยังขาด (`<tests file>`)
## คำแนะนำในการเขียนโค้ด
## เปิด issue แยกไว้แล้ว (ไม่ต้องทำใน PR นี้)

---
ย้ำอีกครั้ง: ทุก **แก้:** ในรีวิวนี้เป็นคำแนะนำในการแก้ ไม่ต้องทำตามทุกบรรทัด ขอแค่ปัญหาของแต่ละข้อหายไป
```

- Every finding names a file and line at the pinned commit, and every blocker or ควรแก้ finding carries a **แก้:** block with code; drop a section left empty.
- Every review opens and closes with the line that the **แก้:** code is a suggestion (the last line of the blockquote, and the closing line after `---`), so the author reads the fixes as advice before and after the findings.

- Plain Thai headings and wording; review jargon ("blocking", "nit", "ฝากไว้") becomes what it means for the author.
- A finding the user defers gets a real issue first (milestone and assignee as the user says), then appears only in the last section as `#N` plus one line. Every deferred finding links to its issue number.
- A finding the user drops leaves the draft entirely.
- Draft to the scratchpad; post with `gh pr review <n> --request-changes|--comment --body-file` once the user says to. Right before posting, list the reviews again and look for one on the same head commit posted while you drafted; if one exists, show it to the user and let them choose which review to keep before you post. To replace your earlier review, overwrite its body instead: `gh api -X PUT repos/{owner}/{repo}/pulls/<n>/reviews/<review id> -F body=@<file>`.

## Answering review comments

1. Read every comment: `gh pr view <n> --json comments,reviews`, plus inline ones via `gh api repos/{owner}/{repo}/pulls/<n>/comments`.
2. Before applying a reviewer's suggestion, trace what it does to behaviour. A suggestion can change meaning while sounding equivalent: `request.user.is_superuser` is not the `super_admin` role in `UserRole`. When it does, take the option that keeps behaviour and say why in the reply.
3. Rerun the tests on `HEAD` after the last commit or merge, and put that real count in the reply, the same as the **ทดสอบ** rule. A count copied from an earlier comment goes stale.
4. Reply in Thai with `gh pr comment <n> --body-file`: what changed, which suggestion you took or declined and why, and what other branches must change when this merges.
