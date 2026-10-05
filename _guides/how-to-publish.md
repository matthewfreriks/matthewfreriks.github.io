# How to publish on matthewfreriks.com

My step-by-step guide for writing in Obsidian and publishing with Jekyll and GitHub Pages.

This file lives in `_guides`. Jekyll ignores folders that start with an underscore, so it never appears on the website.

---

## The workflow at a glance

1. Write the post in `_drafts` (private).
2. Preview it locally with `jekyll serve --drafts`.
3. Move it to `_posts` with a dated file name.
4. Commit and push.
5. Check the live site.

---

## 1. Start a new post

1. In Obsidian, create a new note in the `_drafts` folder.
2. Name it with a short, lowercase title using hyphens, no spaces. Example: `counting-bacteria`
3. Run **Insert template** and choose `post`.
4. Fill in the properties:
    - **title:** the post's headline
    - **subtitle:** one line describing the post (optional)
    - **status:** `published` for a normal post, or `building` for a work-in-progress post

## 2. Write it

- Use `##` for section headings. The post title is already the main heading, so don't use a single `#`.
- Paste or drag images straight into the note. Obsidian saves them in `images/post` automatically.
- **Image file names must not contain spaces.** Rename them with hyphens, like `colony-count-result.png`, and check the link in the note updates.
- Use standard links: `[link text](https://example.com)`

## 3. Preview locally

1. In VS Code, open the terminal in the repository folder.
    
2. Run:
    
    ```
    jekyll serve --drafts
    ```
    
3. Open http://127.0.0.1:4000/blog/ and check the post. Drafts show up here only, never on the live site.
    
4. The preview updates when I save. Refresh the browser to see changes.
    
5. Press **Ctrl + C** in the terminal to stop the server.
    

## 4. Publish it

1. Move the note from `_drafts` to `_posts`.
2. Rename it so it starts with today's date: `YYYY-MM-DD-title`. Example: `2026-12-15-counting-bacteria`
3. If it's a work-in-progress post, set `status: building`. It will appear under **Being built** on the blog page. When it's finished, change it to `published`.

## 5. Commit and push

**In VS Code:**

1. Open the **Source Control** panel.
2. Check the list of changed files makes sense.
3. Write a short message describing the change. Example: `Add counting bacteria post`
4. Click **Commit**, then **Sync Changes**.

**Or in Obsidian**, use the Obsidian Git plugin's commit-and-push command.

## 6. Check the live site

1. Wait a minute or two for GitHub to rebuild the site.
2. Open https://matthewfreriks.com/blog/ and check the post.
3. If something looks old or unstyled, press **Ctrl + F5** to force a full reload.

---

## If something goes wrong

- **The post doesn't appear on the blog page:** check the file is in `_posts`, its name starts with a full date (`2026-12-15-`), and the properties block starts and ends with `---`.
- **An image is broken:** check the file name has no spaces and the image is in `images/post`.
- **A post dated in the future doesn't appear:** Jekyll hides future-dated posts. Fix the date in the file name.
- **`jekyll serve` shows an error:** read the first error line. It usually names the file and line that caused it.
- **The live site didn't update:** on GitHub, open the repository's **Actions** tab to see whether the build failed.

---

## One-time Obsidian setup

The `.obsidian` folder is saved in Git (except `workspace.json`), so these settings are backed up and come across automatically when I clone the repository. This list is a reference in case anything gets reset: