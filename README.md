# Support Trainee Scorecard

An interview scoring dashboard for **Technical Support Trainee** hiring. Interviewers rate 52 standard questions from 0 to 5, and the page calculates the total, the score for each section and a hiring verdict as they go. Saved interviews appear on a Candidates dashboard where you can compare, search and export them.

It is a single HTML file with no build step and no server. Open it in a browser or host it on GitHub Pages.

## Features

- **52 questions in 7 sections:** Introduction & Communication, Basic Computer Knowledge, Basic Troubleshooting, Customer Handling, Logical Thinking, Scenario-Based, and Email & Documentation.
- **Answer guides:** each question has a short note on what a good answer covers, to keep scoring consistent between interviewers.
- **Live scoring:** an animated score ring, a radar chart of the section scores, your strongest section and any weak areas.
- **Focus mode:** shows one question at a time and moves to the next one after each rating.
- **Keyboard scoring:** rate a whole interview without the mouse.
- **Notes and remarks:** a note under any question, plus overall interviewer remarks.
- **Candidates dashboard:** counts for each verdict, search, filters, sorting, and each candidate's section scores against the team average.
- **Export and backup:** a CSV with every question's score, plus JSON backup and restore.
- **Light and dark themes** that follow your system setting, and layouts that work on phones and desktops.

## Scoring rules

| Rating | Meaning   |
|:------:|-----------|
| 5      | Excellent |
| 4      | Very good |
| 3      | Good      |
| 2      | Fair      |
| 1      | Poor      |
| 0      | No answer |

- **Score %** = points earned ÷ (questions rated × 5). Questions you skip don't count against the candidate.
- **Verdict:** 60% or higher is **Recommended**, 45–59% is **Next round**, and below 45% is **Not recommended**.
- Any section scoring under 50% is flagged as a **weak area**.

## Keyboard shortcuts

| Key            | Action                                  |
|----------------|-----------------------------------------|
| `0`–`5`        | Rate the current question and move on   |
| `J` / `K`      | Next / previous question                |
| `←` / `→`      | Previous / next question (focus mode)   |
| `F`            | Turn focus mode on or off               |
| `Esc`          | Leave focus mode                        |
| `G`            | Show or hide the answer guide           |
| `N`            | Add a note to the current question      |

## Getting started

### Open it locally

Download or clone the repository and open `index.html` in any modern browser.

```bash
git clone https://github.com/navgalgotias/Interview-Dashboard-and-Score-validation.git
cd Interview-Dashboard-and-Score-validation
start index.html      # Windows
open index.html       # macOS
xdg-open index.html   # Linux
```

### Publish with GitHub Pages

1. Push the repository to GitHub.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, set **Source** to *Deploy from a branch*, choose `main` and `/ (root)`, and save.
4. After a minute the site is live at `https://<your-username>.github.io/Interview-Dashboard-and-Score-validation/`.

## Where your data is stored

When you open the page from a file or GitHub Pages, interviews are saved in **your browser's local storage**:

- Nothing is sent to a server unless you connect a Google Sheet (see below).
- Data stays on that computer and in that browser. Clearing your browser's site data deletes it.
- Use **Back up (JSON)** on the Candidates tab to keep a copy, and **Restore** to load it in another browser or on another computer. Restoring merges records by ID, so it never creates duplicates.
- A score sheet you haven't saved yet is kept as a draft, so reloading the page doesn't lose it.

When the page is published as a Claude artifact, saved interviews go to the artifact's shared database instead, so everyone the artifact is shared with sees the same candidate list.

## Send results to Google Sheets

The scorecard can write each saved interview to a Google Sheet, one row per candidate:

| Candidate Name | Date | Interviewer | Points | Score | Sections | Verdict | Record ID | Last updated |
|---|---|---|---|---|---|---|---|---|
| Priya Sharma | 2026-09-29 | Navneet | 198/260 | 76.2% | Intro 80% \| Computer 74% \| … | Recommended | c1x9… | 29/09/2026 15:40 |

Saving an interview again updates its row, and deleting it on the dashboard removes the row. **Record ID** is how the scorecard finds a row, so don't edit that column. The scorecard writes row 1 (the headers) and, the first time, formats the tab: a frozen header row, percentages in the Score column, and green, amber or red in the Verdict column.

The page writes to the sheet directly through the Google Sheets API. Each interviewer clicks **Connect Google account** and signs in, and only accounts with edit access to the sheet can write to it.

**Already configured** in `SHEET_DEFAULTS` in `index.html`:

| Setting   | Value |
|-----------|-------|
| Sheet ID  | `1N8eKTMRpRnx0yJzVTafGt8254NSF2mcLkGkBn6g8vrM` |
| Tab name  | `Interview` |
| Client ID | *not set yet (see step 1)* |

### One-time setup: create an OAuth Client ID (about 10 minutes)

Google requires every app that writes to a sheet to have its own Client ID. You only need one for the whole team.

1. Open the [Google Cloud console](https://console.cloud.google.com/) and create a project (or pick an existing one).
2. Go to **APIs & Services → Library**, search for **Google Sheets API**, and click **Enable**.
3. Go to **APIs & Services → OAuth consent screen** (called **Google Auth Platform** in newer consoles):
   - If your team uses Google Workspace, choose **Internal**. Only people in your organisation can sign in, and Google doesn't need to review the app.
   - Otherwise choose **External**, leave it in **Testing**, and add each interviewer's email under **Test users**.
   - Add the scope `https://www.googleapis.com/auth/spreadsheets`.
4. Go to **APIs & Services → Credentials → Create credentials → OAuth client ID**:
   - **Application type:** Web application
   - **Authorized JavaScript origins:** `https://navgalgotias.github.io`, plus `http://localhost:8000` if you want to test on your own computer
5. Copy the **Client ID** (it ends in `.apps.googleusercontent.com`) and paste it into `clientId` in `SHEET_DEFAULTS` in `index.html`. The Client ID isn't a secret, so it's safe to commit to a public repository.
6. In Google Sheets, share the sheet as **Editor** with every interviewer who should be able to send results.

### Using it

1. Open the scorecard from GitHub Pages and go to the **Candidates** tab.
2. In the **Google Sheet** panel, click **Connect Google account** and sign in. The badge changes to **Connected**.
3. Click **Send all interviews** to copy interviews saved earlier into the sheet. From then on, each save and delete is sent automatically. You can turn that off with the checkbox.

Google sign-in lasts about an hour. When it expires, the panel shows **Sign-in needed**. Click **Connect Google account** again, then **Send all interviews** to catch up on anything that wasn't sent.

### Notes

- Google sign-in doesn't work on a page opened directly from a file (`file://`). To test on your own computer, run `python -m http.server 8000` in the project folder and open `http://localhost:8000`.
- Each interviewer can change the Sheet ID, tab name or Client ID in the panel. Their changes are saved in their own browser and override `SHEET_DEFAULTS`.
- Sending to the sheet doesn't work inside the Claude artifact, because that page can't contact Google.

## Customising

Everything lives in `index.html`.

- **Questions and answer guides:** edit the `Q` array. Each entry is `[sectionId, question, answerGuide]`, and question numbers follow the order of the array.
- **Sections:** edit the `CATS` array (`id`, full `name`, and the `short` label used on the radar chart and section tabs).
- **Verdict thresholds:** change `THRESHOLDS` (`rec` for Recommended, `next` for Next round). The verdict, the colours and the tick marks on the score ring all follow it.
- **Colours and fonts:** edit the CSS custom properties at the top of the `<style>` block. Dark-theme values are redefined in the `prefers-color-scheme: dark` and `[data-theme="dark"]` blocks.

## Project structure

```
.
├── index.html              # the whole app: markup, styles and script
├── README.md
├── LICENSE
├── .gitignore
└── .nojekyll               # tells GitHub Pages to serve the files as they are
```

## Browser support

Current versions of Chrome, Edge, Firefox and Safari. The page uses Google Fonts and falls back to system fonts when offline.

## License

[MIT](LICENSE)
