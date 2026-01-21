# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the documentation repository for **Clash Verge Rev**, a cross-platform GUI client for Mihomo (Clash.Meta) built with Tauri. The documentation is built using **MkDocs Material** and is primarily written in **Chinese**.

The main application repository is at: https://github.com/clash-verge-rev/clash-verge-rev

## Development Commands

### Setup
```bash
# Install Python dependencies
pip install -r requirements.txt
```

### Local Development
```bash
# Start local development server
# Listens on http://127.0.0.1:7788 (configured in mkdocs.yml)
mkdocs serve
```

### Build
```bash
# Build static site (generates site/ directory)
mkdocs build
```

## Documentation Structure

### Content Organization
- `docs/` - All documentation content (Markdown files)
  - `index.md` - Homepage with project introduction and features
  - `install.md` - Installation instructions
  - `uninstall.md` - Uninstallation instructions
  - `friendship.md` - Friendly links
  - `guide/` - User guides and tutorials
    - `quickstart.md` - Quick start guide
    - `profile.md` - Subscription/profile management
    - `config.md` - Multi-subscription merge configuration examples
    - `rules.md` - Custom rules
    - `script.md` - Custom scripts
    - `log.md` - Log export
    - `tray_icon.md` - Custom tray icons
    - `css_injection.md` - Custom themes/styles
    - `url_schemes.md` - URL schemes
    - `term.md` - Terminology explanations
    - `group_icon/` - Proxy group icon configuration
  - `faq/` - Platform-specific FAQs
    - `windows.md`, `macos.md`, `linux.md`, `other.md`
  - `assets/` - Images and static resources organized by guide/FAQ sections

### MkDocs Configuration (`mkdocs.yml`)
- **Site name**: Clash Verge Rev Docs
- **Dev server**: Configured to listen on 127.0.0.1:7788
- **Theme**: Material for MkDocs with custom configuration
  - Light/dark mode support
  - Navigation tabs and sections
  - Search with Chinese text segmentation (jieba)
  - Code syntax highlighting
  - Mermaid diagram support
- **Navigation**: Organized by feature area (Introduction, Installation, Guides, Configuration, FAQ)
- **Edit URI**: Points to this repository's main branch for "Edit on GitHub" links

## Key Technical Details

### MkDocs Material Features in Use
- **Markdown extensions**:
  - `admonition` - Warning/info boxes
  - `md_in_html` - Embedded HTML support
  - `pymdownx.details` - Collapsible sections
  - `pymdownx.highlight` - Code highlighting
  - `pymdownx.tabbed` - Tabbed content
  - `pymdownx.superfences` - Mermaid diagrams and code fences
  - `pymdownx.emoji` - Emoji support using Twemoji
- **Navigation features**:
  - Instant loading
  - Navigation tracking and expansion
  - Breadcrumb paths
  - Table of contents following
  - Top navigation return button
- **Search**: Custom separator configuration for Chinese text segmentation

### Content Patterns
- Documentation is written in **Chinese** (Simplified)
- Uses YAML configuration examples for Clash/Mihomo settings
- Heavy use of images stored in `assets/` subdirectories
- Links to external resources (official Mihomo docs, GitHub repos)
- Contains promotional content and sponsorship information

## Deployment

Automated deployment via GitHub Actions (`.github/workflows/deploy.yml`):
1. Triggers on push to `main` or `master` branch
2. Sets up Python 3.x environment
3. Installs dependencies from `requirements.txt`
4. Runs `mkdocs build`
5. Deploys to GitHub Pages

## VSCode Configuration

The repository includes VSCode workspace settings (`.vscode/settings.json`):
- YAML schema validation for `mkdocs.yml` against MkDocs Material schema
- Custom YAML tags for Python name references
- Prettier formatting configuration (double quotes, no trailing commas)

## Content Guidelines

When editing documentation:
- Maintain Chinese language throughout (this is Chinese-language documentation)
- Follow existing image path conventions: `../assets/[section]/[subsection]/[filename]`
- YAML code blocks for Clash/Mihomo configuration should use proper indentation
- Navigation structure is defined in `mkdocs.yml` - update both when adding new pages
- Use Material for MkDocs admonitions for warnings/tips/notes
