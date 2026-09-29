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
- **Verdict:** 75% or higher is **Recommended**, 60–74% is **Next round**, and below 60% is **Not recommended**.
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

- Nothing is sent to a server.
- Data stays on that computer and in that browser. Clearing your browser's site data deletes it.
- Use **Back up (JSON)** on the Candidates tab to keep a copy, and **Restore** to load it in another browser or on another computer. Restoring merges records by ID, so it never creates duplicates.
- A score sheet you haven't saved yet is kept as a draft, so reloading the page doesn't lose it.

When the page is published as a Claude artifact, saved interviews go to the artifact's shared database instead, so everyone the artifact is shared with sees the same candidate list.

## Customising

Everything lives in `index.html`.

- **Questions and answer guides:** edit the `Q` array. Each entry is `[sectionId, question, answerGuide]`, and question numbers follow the order of the array.
- **Sections:** edit the `CATS` array (`id`, full `name`, and the `short` label used on the radar chart and section tabs).
- **Verdict thresholds:** change the `75` and `60` in `verdictFor()`, and move the two tick marks on the score ring to match: they are the `rotate(216 …)` and `rotate(270 …)` lines, where the angle is `percentage × 3.6`.
- **Colours and fonts:** edit the CSS custom properties at the top of the `<style>` block. Dark-theme values are redefined in the `prefers-color-scheme: dark` and `[data-theme="dark"]` blocks.

## Project structure

```
.
├── index.html   # the whole app: markup, styles and script
├── README.md
├── LICENSE
├── .gitignore
└── .nojekyll    # tells GitHub Pages to serve the files as they are
```

## Browser support

Current versions of Chrome, Edge, Firefox and Safari. The page uses Google Fonts and falls back to system fonts when offline.

## License

[MIT](LICENSE)
