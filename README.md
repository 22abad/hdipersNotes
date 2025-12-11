# HDIPERS.LOG 🚀

> **Survival Guide & High Performance Logic for HDip Software Development.**
> No fluff. Just logic.

🔴 **Live Site:** [hdipers.22abad.com](https://hdipers.22abad.com)

## 📖 About
This project is a static site built to serve as a collective knowledge base for our cohort. It hosts revision notes, lab solutions (optimized), and survival guides for the upcoming finals.

**Tech Stack:**
* **Core:** Vanilla HTML/JS (No frameworks, pure speed).
* **Rendering:** `marked.js` for Markdown, `Prism.js` for code highlighting, `Mermaid.js` for diagrams.
* **Hosting:** GitHub + Cloudflare Pages.

---

## 🤝 How to Contribute (Collaboration Protocol)

We welcome contributions! If you have summarized notes or a better solution for a lab, follow these steps to add it to the site.

### Step 1: Fork & Clone
1.  Click the **Fork** button at the top right of this repository.
2.  Clone your forked repo to your local machine:
    ```bash
    git clone [https://github.com/YOUR_USERNAME/hdipersNotes.git](https://github.com/YOUR_USERNAME/hdipersNotes.git)
    ```

### Step 2: Create a New Post
1.  Navigate to the `posts/` folder.
2.  Create a new `.md` file. **Naming Convention:** `YYYYMMDD_Subject_Topic.md` (e.g., `20251212_CS144_TuringMachines.md`).
3.  **Crucial:** You MUST include the **FrontMatter** header at the very top of your file. Copy this template:

```yaml
---
title: Your Post Title Here
title_en: English Title
title_zh: 中文标题 (Optional, or repeat English)
date: 2025-12-11
categories: CS144
tags: Algorithm, Logic, ExamPrep
summary_en: A short description of what this note is about.
summary_zh: 简短的中文描述 (Optional)
---

[EN]
# Your Content Here
Write your notes in standard Markdown.
Code blocks and Mermaid diagrams are supported!
[END]

[ZH]
# 中文内容 (Optional)
如果只想写英文，可以省略这个板块。
[END]

Step 3: Images (Optional)
If you have images, save them in the images/ folder.

In your markdown, link them like this: ![Alt Text](images/my-image.png).

Do not use absolute paths.

Step 4: Push & Pull Request
Commit your changes:

Bash

git add .
git commit -m "Added notes for CSxxx"
git push
Go to your forked repo on GitHub and click "Contribute" -> "Open Pull Request".

I will review and merge it ASAP. The site updates automatically!

🛠 Local Development
To test the site locally:

Clone the repo.

Open index.html in Chrome/Edge/Safari.

Note: Due to browser security policies (CORS), some features might not load if you just double-click the file. It is recommended to use a simple local server (e.g., VS Code "Live Server" extension).

⚖️ License
Knowledge is free. Use it, share it, improve it.