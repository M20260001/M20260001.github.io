# MMI5192E — AI Applications Course Site

This repository is the course website for **MMI5192E**, taught by **Prof. Pin Luarn**
at the **Pforzheim University MBA program**, Winter Term 2026.

The site is hosted on GitHub Pages at **[drluarn.github.io](https://drluarn.github.io/)**
and serves two purposes:

1. **The instructor's personal/course landing page** — who's teaching the course, the
   12 weekly unit topics, and each unit's assignment gallery.
2. **A template every student forks** to build their own personal website as the
   course's hands-on GitHub Pages exercise. Students only need to edit a couple of
   plain-text files and drop in their own images — no HTML/CSS/JS editing required
   for the basic site.

---

## Live pages

| Page | URL |
|---|---|
| Home / instructor profile | <https://drluarn.github.io/> |
| Student portfolio directory | <https://drluarn.github.io/student.html> |
| Unit detail pages | <https://drluarn.github.io/unit.html?unit=1> … `?unit=12` |
| Perception AI demo hub | <https://drluarn.github.io/perception_demo.html> |
| MediaPipe "Info Wall" demo | <https://drluarn.github.io/mediapipeCC.html> |

---

## Directory structure

```
.
├── index.html            Home page (profile, unit grid, links)
├── student.html           Student portfolio directory + photo gallery
├── unit.html               Per-unit assignment gallery (?unit=1..12)
├── style.css                Shared navy/gold theme for the pages above
├── profile.txt                Editable personal info for index.html
├── projects.txt                 The 12 unit titles, read from the instructor's site
├── mypic.jpg                      Profile photo shown on index.html
├── favicon.ico                     Site icon (Dr. Luarn's cartoon mascot)
├── perception_demo.html    Landing page for the four AI perception demos
├── mediapipeCC.html         Standalone MediaPipe hand/pose "info wall" demo
├── chatbot.js                Optional n8n-backed chat widget (currently disabled)
├── mp/                         Four browser-only AI perception demos
│   ├── face.html                 MediaPipe Face Mesh (468 landmarks)
│   ├── hand.html                   MediaPipe Hands (21 landmarks)
│   ├── gesture.html                  MediaPipe hand-gesture recognition
│   └── object.html                     TensorFlow.js COCO-SSD object detection
├── pic/                        Unit assignment screenshots (unit{N}_{i}.*)
├── URL/                          Captions/demo links for each pic/ image
└── p1/                             Photos shown in student.html's Photo Gallery
```

---

## File-by-file guide

### Core pages

- **`index.html`** — The course/instructor home page. Hero section, About,
  the 12-unit project grid, student-portfolio call-to-action, and contact info.
  All of the editable text is pulled at runtime from `profile.txt` and
  `projects.txt` via `fetch()`, so this file itself rarely needs to change.
  English-only (no language switcher).

- **`student.html`** — Lists every student's personal site. The grid is
  populated by reading a Google Sheet (via its public JSON endpoint) that the
  class fills in with name / student ID / website URL. Includes:
  - a search box to filter by name or ID,
  - a "Reload from Google Sheet" button,
  - a "Submit My Student ID & Website URL" button linking to the class
    Google Form,
  - a **Photo Gallery** section at the bottom that shows the images in `p1/`
    (click a thumbnail to open it in a lightbox with Prev/Next navigation).

- **`unit.html`** — Shows one unit's assignment gallery, selected via
  `?unit=1` … `?unit=12` in the URL. It:
  - reads the unit's title from `projects.txt` (always fetched from
    `https://drluarn.github.io/projects.txt`, so every student fork shows the
    same, current unit list regardless of whose repo it's hosted on),
  - auto-detects however many `pic/unit{N}_{i}.*` images exist for that unit
    (accepts `.jpg`/`.JPG`/`.jpeg`/`.JPEG`/`.png`/`.PNG`),
  - reads an optional caption/demo-link from the matching `URL/URL{N}_{i}.txt`
    file for each image,
  - has Prev/Next unit navigation and left/right arrow-key shortcuts.

- **`style.css`** — The shared visual theme (deep navy/purple gradient with a
  rose-gold accent) used by `index.html`, `student.html`, and `unit.html`.

### Student-editable data files

- **`profile.txt`** — Plain `key=value` lines for everything personal shown on
  `index.html`: name, titles, expertise, education, email, LinkedIn, the four
  "About Me" lines, phone, and location. This is the **one file every student
  needs to edit** to turn this template into their own personal page — no code
  changes required. Lines starting with `#` are comments.

- **`projects.txt`** — `unit1=` … `unit12=` lines with the course's 12 weekly
  topics. This file is **instructor-maintained**: `index.html` and `unit.html`
  always fetch it from `https://drluarn.github.io/projects.txt`, not from the
  student's own fork, so the unit list stays in sync across every student site
  automatically. A copy still ships in each fork purely as the offline fallback
  used if that fetch fails.

### Images

- **`mypic.jpg`** — The profile photo shown in the hero section of
  `index.html`. Replace with your own photo (same filename).

- **`favicon.ico`** — The browser-tab icon: Dr. Luarn's cartoon mascot avatar.

- **`pic/`** — Screenshots/photos for each unit's assignment gallery, named
  `unit{N}_{i}.<ext>` (e.g. `unit1_1.jpg`, `unit1_2.png`). `unit.html` detects
  however many exist per unit automatically. `pic/no.jpg` is the placeholder
  shown when a unit has no images yet.

- **`URL/`** — One `URL{N}_{i}.txt` file per image in `pic/` (e.g.
  `URL/URL1_1.txt` describes `pic/unit1_1.*`). Format:
  ```
  <caption text>
  <optional demo URL>
  ```
  The first line replaces the image's caption; the second line (optional)
  becomes a clickable "demo" link under the image.

- **`p1/`** — Photos for the **Photo Gallery** section on `student.html`,
  named `p1.jpg`, `p2.jpg`, … (any of `.jpg/.JPG/.jpeg/.JPEG/.png/.PNG`).
  Numbers don't need to be contiguous — any gaps are simply skipped.

### AI perception demos (MediaPipe / TensorFlow.js)

All demos run entirely client-side in the browser — no server, no data leaves
the device.

- **`perception_demo.html`** — A small landing page linking to the four demos
  below, styled to match the rest of the site.

- **`mp/face.html`** — MediaPipe Face Mesh: 468 facial landmarks, with a
  toggle between the full tessellated mesh and a simpler contour view.

- **`mp/hand.html`** — MediaPipe Hands: up to 2 hands, 21 landmarks each,
  color-coded per finger.

- **`mp/gesture.html`** — Builds on hand tracking to recognize 8 gestures
  (fist, open palm, pointing up/down, thumbs up/down, victory, "I love you").

- **`mp/object.html`** — TensorFlow.js + COCO-SSD object detection (80 object
  classes), with a **Switch Camera** button that appears automatically when
  the device has more than one camera (e.g. a phone's front/back cameras),
  so you can point the rear camera outward while watching live detections.

  All four demos pre-load their model in the background as soon as the page
  opens (instead of waiting for the camera to start), show a small "loading
  model" badge over the video while that finishes, and play a gold light
  flash + a short synthesized chime the moment detection actually goes live.

- **`mediapipeCC.html`** — A standalone, full-screen "info wall" demo:
  MediaPipe hand + pose tracking driving a particle animation (shapes/colors
  switch on a fist→open-palm gesture) alongside a live clock and info panel.

### Optional / supporting

- **`chatbot.js`** — A floating chat-widget class that talks to an n8n
  webhook. It's currently commented out in `index.html` (`<!-- <script
  src="chatbot.js"></script> -->`); uncomment that line and set your own
  webhook URL inside the file to enable it.

---

## For students: turning this fork into your own site

> **Full step-by-step walkthrough:** [STUDENT_SETUP.md](STUDENT_SETUP.md)
> (how to use the "Use this template" button, name your repo, and turn on
> GitHub Pages). Quick summary:

1. Click **"Use this template"** on this repo to create your own, independent
   copy, then enable GitHub Pages on it.
2. Edit **`profile.txt`** with your own name, titles, education, contact info,
   and About Me text.
3. Replace **`mypic.jpg`** with your own photo (keep the filename).
4. For each unit you complete, drop your screenshot(s) into **`pic/`** as
   `unit{N}_1.jpg`, `unit{N}_2.jpg`, … and optionally add a matching
   **`URL/URL{N}_{i}.txt`** with a caption and a demo link.
5. Submit your student ID and site URL through the Google Form linked from
   `student.html`, so you show up in the class directory.

You do **not** need to edit `projects.txt`, `index.html`, `student.html`,
`unit.html`, or `style.css` — those are either instructor-maintained or just
work once `profile.txt` is filled in.
