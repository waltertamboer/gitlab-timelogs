# GitLab Timelogs

A single-file browser app for reviewing and logging time on GitLab issues and merge requests. No build step, no dependencies — open `index.html` directly in your browser.

![Login](/screenshot-login.png)

## Running via Docker

Pull and run the pre-built image:

```bash
docker run -p 8080:80 ghcr.io/waltertamboer/gitlab-timelogs:latest
```

Then open `http://localhost:8080` in your browser.

Or with Docker Compose — create a `docker-compose.yml`:

```yaml
services:
  gitlab-timelogs:
    image: ghcr.io/waltertamboer/gitlab-timelogs:latest
    ports:
      - "8080:80"
    restart: unless-stopped
```

Then run:

```bash
docker compose up -d
```

## Setup

1. Open `index.html` in your browser (or the Docker URL above).
2. Create a GitLab Personal Access Token with the `api` scope at **gitlab.com → User Settings → Access Tokens**.
3. Enter your token, your GitLab username, and the API base URL (defaults to `https://gitlab.com`; change this for self-hosted instances).
4. Click **Save & continue**.

Your credentials are stored only in your browser's `localStorage` and never sent anywhere other than the GitLab API.

## How to use

### Navigating days

Use the **‹ / ›** arrows to move one working day back or forward (weekends are skipped). The date picker lets you jump to any date, and **Today** returns to the current day. Click **Load** to (re)fetch data for the selected date.

### Calendar view

Click **Calendar** to toggle a monthly overview. Each cell shows the hours logged that day, colour-coded:

- **Green** — 8 h or more
- **Orange** — partial hours logged
- **Red** — nothing logged

Click any cell to jump to that day.

### Day types

Mark a day as **Sick day** or **Vacation** using the toolbar buttons. A banner is shown as a reminder that no time logging is required. The setting is stored locally and visible in the calendar.

### Time logged

The **Time logged** section shows all time entries for the selected day, grouped by issue or MR. Each group can be expanded to see individual entries with their duration and summary. Entries can be removed with the **Remove** button.

### Activity

The **Activity** section shows all GitLab events for the day (pushes, issue updates, MR activity, comments) in chronological order.

### Unlogged issues

After loading, the app analyses your activity to detect issues or MRs you worked on that have no time logged yet. These are listed in the **Issues worked on with no time logged** callout. Detection is based on:

- Branch names that start with an issue IID (e.g. `123-fix-bug`)
- MR events referencing a source branch tied to an issue
- Comments left on issues
- Direct issue events (opened, closed, etc.)

Items marked **Review only** are MRs where you were not the author (no pushes to the branch detected), suggesting review work rather than development.

### Logging time

When unlogged issues are detected, a **Log time** card appears above them. For each issue or MR:

1. Set the number of hours using the **−/+** buttons or by typing directly into the field.
2. Optionally add a summary.
3. Click **Distribute evenly** to split 8 h across all items (review-only items are capped at 0.5 h each).
4. Click **Submit** to create time entries in GitLab via the API.

### Pinned issues

Pin any issue to keep it visible across all days. Pinned issues appear at the top of the page and can be used to quickly log time on recurring work by clicking **+ Log time**, which adds the issue to the log time card.

### Issue detail panel

Click any issue or MR title in the log time card, unlogged list, or pinned issues to open a side panel showing the title, metadata (author, assignees, state, labels, milestone), and description. The panel has an **Open in GitLab ↗** button to go directly to the item in GitLab. Close it with **✕**, by clicking the overlay, or by pressing `Escape`. Ctrl/Cmd+click on a title goes directly to GitLab without opening the panel.
