# How my website is organised

A map of every folder and file in my website repository, what each one does, and how they connect.

This file lives in `_notes/guides`. Folders starting with an underscore are never published, so it stays private to the repository.

---

## The big picture

My repository holds the **ingredients**. Jekyll combines them into finished web pages.

1. I write content (posts and projects) in **Obsidian**.
2. **Jekyll** wraps that content in layouts, adds the navigation and footer, and builds complete pages into `_site`.
3. When I push to GitHub, **GitHub Pages** runs Jekyll on its own servers and publishes the result at matthewfreriks.com.

---

## The folder tree

```text
matthewfreriks.github.io/
│
├── index.html            Homepage
├── lab.html              Lab work and skills page
├── projects.html         Projects page
├── blog.html             Blog page
├── 404.html              "Page not found" page
│
├── content/              ALL MY WRITING
│   ├── _posts/           Published blog posts
│   ├── _drafts/          Unpublished posts
│   └── _projects/        One file per project
│       ├── planned/
│       ├── in-progress/
│       └── complete/
│
├── _layouts/             Page templates
├── _includes/            Reusable pieces (navigation, footer)
├── _data/                Data the pages read (skills)
├── _config.yml           Site-wide settings
│
├── _notes/               MY PRIVATE NOTES (never published)
│   ├── guides/           How-to guides, like this one
│   └── templates/        Obsidian templates
│
├── images/               All pictures
├── files/                Public CV PDF
├── style.css             All styling
├── favicon.ico           Tab icon
├── CNAME                 My domain name (don't delete)
├── sitemap.xml           List of pages for search engines
├── robots.txt            Instructions for search engines
│
├── _site/                Jekyll's finished website (generated)
├── .jekyll-cache/        Jekyll's scratch space (generated)
│
├── .git/                 Git's version history
├── .gitignore            What Git should ignore
├── .obsidian/            Obsidian settings
└── .vscode/              VS Code settings and auto-start tasks
```

---

## Pages people visit

Each of these becomes a page on the site. They only hold the content unique to that page; the shared parts come from layouts and includes.

| File            | Address         | What it shows                                                                 |
| --------------- | --------------- | ----------------------------------------------------------------------------- |
| `index.html`    | /               | Hero, About, Experience, featured projects, science communication, leadership |
| `lab.html`      | /lab/           | Lab photo gallery and skill boxes                                             |
| `projects.html` | /projects/      | Every project, grouped by status                                              |
| `blog.html`     | /blog/          | "Being built" posts, then all finished posts                                  |
| `404.html`      | Any broken link | Friendly "page not found" with links home                                     |

---

## `content/`: my writing

This is where I spend most of my time. Everything here is Markdown, edited in Obsidian.

### `_posts/`: published blog posts

- File names **must** start with the date: `2026-12-15-counting-bacteria.md`
- The date in the name becomes the post's date and part of its address: /blog/2026/counting-bacteria/
- `status: building` puts a post under "Being built" on the blog page.
- `project: file-name` adds it to that project's "Build log".

### `_drafts/`: unpublished posts

- Never appear on the live site.
- Preview them locally with `jekyll serve --drafts`.
- No date needed in the file name until I move them to `_posts`.
- **Remember:** the repository is public, so anyone browsing it on GitHub can read drafts. Nothing private goes here.

### `_projects/`: one file per project

- **The folder sets the status:** `planned`, `in-progress` or `complete`. Moving a file between folders updates the site.
- The **file name** becomes the address: `counting-bacteria.md` → /projects/counting-bacteria/
- File names must be unique across all three folders.
- Key properties:
  - `order`: position in project lists (1 = first)
  - `featured: true`: shows on the homepage (keep to 3 or 4)
  - `skills`: shown on the page, only what **I** did
  - `tags`: link the project to my skills page and related posts

---

## Jekyll's building blocks

These folders must stay at the top level with these exact names.

### `_layouts/`: page templates

Layouts wrap content. They stack inside each other:

```text
default.html     the outer shell of EVERY page:
                 <head>, fonts, favicon, preview card, footer
   │
   ├── post.html      wraps blog posts: header with date and title, back link
   └── project.html   wraps projects: status label, skills box, build log
```

So a project page = my project file → inside `project.html` → inside `default.html`.

### `_includes/`: reusable pieces

| File                | Used in                         | What it is                                       |
| ------------------- | ------------------------------- | ------------------------------------------------ |
| `nav.html`          | Every page header               | The top navigation menu. **Add new pages here.** |
| `footer.html`       | Every page (via `default.html`) | "Let's talk" section and platform buttons        |
| `status-label.html` | Projects                        | Turns `done` into "Complete", and so on          |

Changing one of these changes it everywhere at once.

### `_data/`: data files

| File | Used by | What it holds |
|---|---|---|
| `skills.yml` | `lab.html` | Skill groups, their items, and the tags that link them to projects |

### `_config.yml`: site-wide settings

Holds my site title and address, the blog and project URL patterns, `collections_dir: content`, the folder-based project statuses, and the list of files to exclude.

**Jekyll only reads this at startup.** After changing it, stop the server (Ctrl + C) and run `jekyll serve` again.

Indentation must use **spaces, never tabs**, and lines that belong to a section are indented under it.

---

## `_notes/`: my private notes

| Folder | What it holds |
|---|---|
| `guides/` | How-to guides: publishing, this structure guide, my tag vocabulary |
| `templates/` | Obsidian templates: `post.md`, `build-log.md`, `project.md` |

Obsidian's template folder setting must point to `_notes/templates`.

---

## Files copied to the site as they are

| File or folder | Notes |
|---|---|
| `style.css` | All styling. Colours and fonts are set once at the top in `:root`. |
| `images/` | `labwork_skills/` for the lab page, `post/` for blog images, `projects/` for project images. Plus the favicon images, homepage microscope image, lab header and preview card. **No spaces in file names.** |
| `files/` | `Matthew-Freriks-CV.pdf`, the **public** CV with no phone numbers. Replace it with the same name when updating. |
| `favicon.ico` | Tab icon |
| `CNAME` | Tells GitHub my domain is matthewfreriks.com. **Deleting it breaks my domain.** |
| `sitemap.xml` | Built automatically from all pages and posts |
| `robots.txt` | Points search engines to the sitemap |

---

## Generated folders: never edit

| Folder | What it is |
|---|---|
| `_site/` | The finished website Jekyll builds. It's rebuilt every time I save, so edits here vanish. Ignored by Git, and safe to delete. |
| `.jekyll-cache/` | Speeds up Jekyll's builds. Ignored by Git, and safe to delete. |

---

## Tool settings

| File or folder | Purpose | Saved in Git? |
|---|---|---|
| `.git/` | The full history of every change. **Never touch it.** | It is Git |
| `.gitignore` | Lists what Git ignores: `_site`, `.jekyll-cache`, `.obsidian/workspace.json`, `.vs` | Yes |
| `.obsidian/` | Obsidian settings (link format, attachment folder, templates) | Yes, except `workspace.json` |
| `.vscode/tasks.json` | Auto-starts Jekyll, Obsidian and the browser when I open the folder | Yes |
| `.vscode/settings.json` | Hides `_site`, `.jekyll-cache` and `.git` from VS Code's file list | Yes |

---

## How things connect

**Tags** are the glue. One project's tags can link it to:

- skill boxes on the lab page (via `_data/skills.yml`)
- related posts and projects (and, later, the connection graph)

**`project:` in a post** links that post into the project's "Build log".

**`featured: true`** puts a project on the homepage.

**The folder** a project sits in sets its status everywhere it appears.

---

## Quick reference: where does it go?

| I want to... | Do this |
|---|---|
| Start a blog post | New note in `content/_drafts`, insert the `post` template |
| Write a project update | New note in `content/_drafts`, insert the `build-log` template, set `project:` |
| Publish a post | Move it to `content/_posts`, rename to `YYYY-MM-DD-title` |
| Add a project | New note in the right `content/_projects` folder, insert the `project` template |
| Mark a project finished | Drag it into `content/_projects/complete` |
| Show a project on the homepage | Set `featured: true` |
| Add a skill group | Copy a block in `_data/skills.yml` |
| Add a page to the menu | Add a line to `_includes/nav.html` |
| Change a colour or font | Edit the `:root` section at the top of `style.css` |
| Change the footer buttons | Edit `_includes/footer.html` |
| Update my CV | Replace `files/Matthew-Freriks-CV.pdf`, same name |
| Change a site setting | Edit `_config.yml`, then restart Jekyll |

---

## If something breaks

- **Jekyll shows an error:** read the first error line. It names the file and usually the line.
- **A page or project is missing:** check the file is in the right folder, its properties block starts and ends with `---`, and (for posts) the file name starts with a full date.
- **An image is broken:** check the path starts with `/images/`, the folder name matches exactly, and the file name has no spaces.
- **Styling looks old:** press Ctrl + F5.
- **The live site didn't update:** check the **Actions** tab on GitHub for a failed build.
