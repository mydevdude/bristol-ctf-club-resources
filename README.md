# Bristol CTF Club Resources

A collaborative knowledge base for Bristol Cybersecurity Club CTF success built with [Quartz](https://quartz.jzhao.xyz/) from content authored in [Obsidian](https://obsidian.md/)

**Live site:** [bristolctf.club](https://bristolctf.club)


# Beginner's Guide to Contributing

Thank you for your interest in contributing! This guide will help you get started.

## Content Contribution Guide

First, you'll need to have git installed on your machine.
You will also need [Obsidian](https://obsidian.md/) unless you want to edit in markdown files directly.

### Step 1: Fork the Repository

1. Go to the repository on GitHub
2. Click the **"Fork"** button in the top-right corner
3. This creates a copy of the repo under your GitHub account

### Step 2: Clone Your Fork

From your fork, click the green "Code" button and copy the url.

Make sure you have git installed on your machine.

In your terminal, clone your forked repository:
```bash
git clone <paste link here>
#git clone https://github.com/YOUR-USERNAME/bristol-ctf-resources.git
```
this will download the entire repository to a new directory, bristol-ctf-resources.

### Step 3: Create a Branch for Your Edits

```bash
git checkout -b myEdits
```

### Step 4: Open in Obsidian

1. Install [Obsidian](https://obsidian.md/) if you haven't already
2. Open Obsidian and select **"Open folder as vault"**
3. Navigate to your cloned repo and select the `content/` folder
4. Edit and create new notes or folders in Obsidian


### What Makes a Good Contribution?

Good contributions include new notes, tips, tricks, resources, guides, tutorials, etc.

In an effort to avoid an overwhelming amount of information, the goal is to initially keep it to the basic foundations of how to do things.

If you want to go into greater detail, please make a separate note in the appropriate folder.

Once you've made your edits, see [Submitting Your Contribution](#submitting-your-contribution) below.


## Submitting Your Contribution

Once you've made your changes locally, follow these steps to submit them:

### Step 1: Save and Commit Your Changes

```bash
# Stage all your changes
git add .

# Commit with a descriptive message
git commit -m "Add guide for SQL injection basics"
```

### Step 2: Push to Your Fork

```bash
git push -u origin myEdits
```

This uploads your changes to **your fork** on GitHub (not the original repository).

### Step 3: Open a Pull Request

1. Go to **your fork** on GitHub (github.com/YOUR-USERNAME/bristol-ctf-resources)
2. You'll see a banner saying "myEdits had recent pushes" with a **"Compare & pull request"** button — click it
3. If you don't see the banner, click the **"Contribute"** dropdown and select **"Open pull request"**
4. Fill out the pull request form:
   - **Title**: Brief description (e.g., "Add SQL injection basics guide")
   - **Description**: Explain what you added or changed
5. Click **"Create pull request"**

### Step 4: Wait for Review

A maintainer will review your PR. They may:
- **Approve and merge it** — your changes go live!
- **Request changes** — make the requested edits, commit, and push again (the PR updates automatically)
- **Ask questions** — respond in the PR comments


## Styling and UI Contributions

To work on or experiment with improvements to the site's UI and styling, you will need to install dependencies and run the quartz server locally:

### Setting Up Your Development Environment

#### Prerequisites: Install Node.js

Node.js is required to run the development server. Download and install it from [nodejs.org](https://nodejs.org/) (use the LTS version).

Verify installation:
```bash
node --version  # Should show v22 or higher
npm --version   # Should show v10 or higher
```

#### 1. Fork and Clone (same as Content Contributions above)

Follow Steps 1-3 from the Content Contribution Guide to fork, clone, and create a branch.

#### 2. Install Dependencies

```bash
npm install
```

#### 3. Start the Dev Server

```bash
npx quartz build --serve
```

Visit `http://localhost:8080` to preview your changes. The server auto-reloads when you save files.


### How Quartz Styling Works

Quartz uses a combination of configuration files, layout definitions, and SCSS for styling. Here's a breakdown:

#### Key Files Overview

| File | Purpose |
|------|---------|
| `quartz.config.ts` | Site settings: title, colors, fonts, plugins |
| `quartz.layout.ts` | Page structure: which components appear where |
| `quartz/styles/custom.scss` | Your custom CSS overrides |
| `quartz/components/` | React components (search, explorer, etc.) |

#### 1. Theme Colors (`quartz.config.ts`)

The `theme.colors` section defines color variables for light and dark modes:

```typescript
colors: {
  darkMode: {
    light: "#161618",      // Page background
    lightgray: "#393639",  // Borders, dividers
    gray: "#646464",       // Subtle text, icons
    darkgray: "#d4d4d4",   // Body text
    dark: "#ebebec",       // Headings, bold text
    secondary: "#7b97aa",  // Links, accents
    tertiary: "#84a59d",   // Hover states
    highlight: "rgba(...)",// Selection highlight
  },
}
```

These colors are available as CSS variables: `--light`, `--dark`, `--secondary`, etc.

#### 2. Typography (`quartz.config.ts`)

Change fonts in the `theme.typography` section:

```typescript
typography: {
  header: "Schibsted Grotesk",  // Headings
  body: "Source Sans Pro",       // Body text
  code: "IBM Plex Mono",         // Code blocks
},
```

#### 3. Page Layout (`quartz.layout.ts`)

This file controls which components appear on each page:

```typescript
export const defaultContentPageLayout: PageLayout = {
  beforeBody: [              // Above the main content
    Component.Breadcrumbs(),
    Component.ArticleTitle(),
  ],
  left: [                    // Left sidebar
    Component.PageTitle(),
    Component.Search(),
    Component.Explorer(),
  ],
  right: [                   // Right sidebar
    Component.TableOfContents(),
    Component.Backlinks(),
  ],
}
```

To remove a component, delete or comment out its line. To add one, import it from `Component.*`.

#### 4. Custom CSS (`quartz/styles/custom.scss`)

For fine-grained control, add custom SCSS here. Example:

```scss
// Change explorer sidebar background
body .sidebar.left:has(.explorer) {
  background-color: #0d2e4d;
}

// Style folder titles
a.folder-title {
  color: #adbd96 !important;
}
```

**Tip:** Use browser DevTools to inspect elements and find the right selectors.

#### 5. Component Styling (`quartz/components/styles/`)

Each component has its own `.scss` file. For example, to modify the search bar, edit `quartz/components/styles/search.scss`.

#### Resources

- [Quartz Configuration Docs](https://quartz.jzhao.xyz/configuration)
- [Quartz Layout Docs](https://quartz.jzhao.xyz/layout)
- [Creating Custom Components](https://quartz.jzhao.xyz/advanced/creating-components)



### Tips for First-Time Contributors

- Don't worry about making mistakes — that's what review is for!
- If you get stuck, open an issue and ask for help
- You can make multiple commits to the same branch; they'll all be included in your PR


---

## Questions?

- Open an issue for questions or suggestions
- Check existing issues before creating new ones

Thank you for contributing! 🎉

