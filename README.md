# ABIM Maintainer Workflow

The interactive maintainer workflow for the AnKing ABIM Anki deck — a clickable
flowchart over the full stage-by-stage checklists.

**Live page:** https://beejumm.github.io/abim-maintainer-workflow/

---

## How to update it

The whole site is one file: **`index.html`**. No build step, no dependencies,
no framework. Edit it, commit, and GitHub Pages redeploys within a minute.

You can edit it three ways:

- **In the browser** — open `index.html` here on GitHub and click the pencil
  icon. Fine for changing a link or fixing a typo.
- **In `github.dev`** — press `.` on the repo page. Full editor, no install.
- **Locally** — clone the repo, edit, push. Open `index.html` directly in a
  browser to preview; there is no server to run.

## Link targets

Every external URL the page uses lives in one block near the bottom of
`index.html`, in the `<script>` tag:

```js
var LINKS = {
  ankihubDeck: { url: "https://app.ankihub.net/", label: "AnkiHub deck browser" },
  community:   { url: "https://community.ankihub.net/", label: "AnkiHub Community forum" },
  tagsFile:    { url: null, label: "The ABIM blueprint tag list" },
  styleGuide:  { url: null, label: "The card creation style guide" }
};
```

A `url` of `null` is not a broken link — it renders as a dashed
`— link not set` placeholder and lists itself in the **Links still to set**
panel at the bottom of the page. Fill in the URL and the placeholder becomes a
live chip everywhere it appears.

To add a new link target: add an entry here, then reference it in the HTML with
`<span data-link="yourKey" data-text="Label"></span>`.

## Editing the flowchart

Flowchart boxes are plain HTML in the `<div class="flow">` block. Each one
carries a `data-node="id"`, and the connecting arrows are drawn from the
`EDGES` array in the script:

```js
var EDGES = [
  ["start", "s0"], ["s0", "s1"], ["s1", "q1"],
  ["q1", "a", "YES"], ["q1", "b", "NO"],
  ...
];
```

Each entry is `[fromNode, toNode, optionalArrowLabel]`. The arrows are computed
from where the boxes actually land, so adding or reordering a box does not
require touching any coordinates — add the box, add its edges, done.

## Checkbox state

Ticks are stored in each maintainer's own browser (`localStorage`), never on a
server and never shared. Changing the `id` of a checkbox resets it for everyone,
so leave existing ids alone when editing surrounding text.

## Conventions this page encodes

- Every card carries a source QID tag: `#AK_ABIM::MKSAP::abxyz12345`
- Blueprint tags: `#AK_ABIM::!Specialties::<Subject>::NN_<Topic>::NN_<Subtopic>`
- Approval needs upvotes from `floor(maintainers ÷ 2)` — odd numbers round down
- Nobody merges their own suggestion

Discussion and voting happen on [AnkiHub Community](https://community.ankihub.net/).
