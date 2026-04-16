# FIN 4329 Test 2 Quiz — Setup & Deployment

## What This Is

A self-hosted, GitHub-Pages-deployed online quiz for FIN 4329 Bank Lending Test 2. 20 multiple-choice questions, auto-graded, two attempts, results emailed to instructor.

## Student-Facing Features

- **Access gate**: Students must enter name, `@miners.utep.edu` email, and the access password (`[REDACTED — given to instructor only]`).
- **20 multiple-choice questions** covering Chapters 6, 7, 8 — all fresh, not repeated from review sessions.
- **Numerical questions** include adjusted net worth, self-employment tax, installment sale gain, Section 179 phase-out, depreciation recapture, AMT, Section 1250 tax, partnership income allocation, and at-risk basis.
- **Scoring**: Pass = 80%+, Fail = <80%.
- **Two attempts**: Attempt 1 until 1:20 PM MT April 16, 2026. Attempt 2 (only wrong questions) until 11:59 PM MT April 16, 2026. Final score = 50% Attempt 1 + 50% Attempt 2.
- **Review screen** shows which questions were wrong with full explanations.

## Anti-Cheat Measures

- Fullscreen mode required (auto-entered, exits logged)
- Tab-switch detection (each switch logged with timestamp)
- Right-click, copy, paste, print, F12 disabled
- Back-button resubmit prevented
- `noindex, nofollow, noarchive` meta tags (not indexed by Google)
- Progress saved to browser localStorage (cannot switch browsers between attempts)
- All violations emailed to instructor in results

**Disclaimer**: A web page cannot fully lock down a browser. For true lockdown, use Respondus LockDown Browser through UTEP's LMS. These measures are best-effort.

---

## FERPA Compliance

**No third-party service ever touches student grades.** Student data is never transmitted anywhere. Instead, students download a JSON results file and upload it to a Blackboard assignment — which is UTEP's official, FERPA-compliant LMS.

## Submission Flow (FERPA-Safe)

1. Student finishes each attempt.
2. Quiz shows a **prominent download button** on the results page.
3. Student downloads a JSON file named `FIN4329_Test2_[Name]_[Score]pct.json`.
4. Student uploads that file to the Blackboard assignment you create.

**On your end**: Create a Blackboard assignment titled "Test 2 Quiz Results" (or similar), set "File Upload" as the submission type. Collect JSON files. Each file contains: name, email, both attempts with timestamps and answers, final weighted score, all proctoring violations with timestamps.

### Optional: Grade by opening the JSON

Each JSON has this structure (human-readable):
```json
{
  "name": "Jane Doe",
  "email": "jdoe@miners.utep.edu",
  "attempt1": { "correctCount": 12, "total": 20, "scorePct": 60.0, "timestamp": "...", "violations": [] },
  "attempt2": { "correctCount": 6, "total": 8, "scorePct": 75.0, "timestamp": "...", "violations": [] },
  "finalPct": 67.5,
  "finalLabel": "...",
  "locked": true
}
```

## Attempt Lock (no retakes)

- After Attempt 2 is submitted, the state is **locked** — that student cannot access the quiz again, only see their results and re-download the file.
- After 12:00 AM MT on April 17, 2026, the **entire quiz is killed for all users** (hard kill date). The URL will show "Quiz Permanently Closed."
- GitHub Pages itself will also be unpublished automatically at 12:15 AM MT on April 17, 2026 (scheduled task). The URL will 404.

### Deploy to GitHub Pages

Run from this folder (in Git Bash or Terminal):

```bash
cd "D:/Dropbox/UTEP/Spring 2026/FIN4329-Bank Lending/Improved_PPTs/outputs/quiz-test2"
git init
git add .
git commit -m "Initial commit: FIN 4329 Test 2 Quiz"
gh repo create fin4329-test2-quiz --public --source=. --remote=origin --push
gh api --method POST /repos/:owner/fin4329-test2-quiz/pages -f "source[branch]=main" -f "source[path]=/"
```

Wait 1–2 minutes, then your quiz will be live at:
`https://<YOUR_GITHUB_USERNAME>.github.io/fin4329-test2-quiz/`

### Step 3: Share with students

Give students:
- The URL
- Password: **`[REDACTED — given to instructor only]`**
- Reminder to use their `@miners.utep.edu` email

---

## Admin / Troubleshooting

**Change the access code**: Edit `ACCESS_CODE = "[REDACTED — given to instructor only]";` in `index.html`, commit and push.

**Change deadlines**: Edit `ATTEMPT1_DEADLINE_ISO` and `ATTEMPT2_DEADLINE_ISO` at the top of the `<script>` block. Format: `YYYY-MM-DDTHH:MM:SS-06:00` (Mountain Daylight Time is UTC-6).

**Students see wrong score / progress wiped**: localStorage lives in the browser. If they clear cookies or switch browsers, their progress is lost. Tell them to use the SAME browser for both attempts.

**Email not arriving**: Check your Formspree dashboard for submissions. If Formspree works but email isn't arriving, check spam folder. Formspree delivers from their domain.

**Preventing early viewers from indexing**: Already handled via `noindex` meta tag. You can also add `robots.txt` disallowing all paths if desired.

## Files

- `index.html` — the quiz itself (all logic, questions, styles in one file)
- `README.md` — this file

Good luck with Test 2!
