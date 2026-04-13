# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an academic project page for "Any House Any Task (AHAT)" - a research paper on long-horizon task planning for household robotics. The page is built as a static HTML/CSS/JavaScript site using the [Academic Project Page Template](https://github.com/eliahuhorwitz/Academic-project-page-template).

**Key characteristics:**
- No build system or bundler
- No package manager (no npm/yarn)
- Static files served directly
- Designed for GitHub Pages deployment

## Architecture

### Core Structure

```
index.html              # Main page with all content
static/
├── css/               # Bulma CSS framework + custom styles
├── js/                # jQuery, Bulma plugins, custom JS
├── images/            # Paper figures, screenshots, favicon
├── videos/            # Demo videos, presentations
├── pdfs/              # Paper PDFs
└── demos/
    ├── data.json      # Interactive demo data
    └── videos/        # Demo execution videos
```

### Interactive Demo System

The page includes **two separate interactive demos** at `static/js/interactive-demo.js`:

**Demo 1 - Model Output Only:**
1. Allows selection of scene graph and instruction
2. Displays model output and execution plan
3. Does NOT display video
4. Uses element IDs: `scene-graph-select-1`, `instruction-select-1`, `demo-output-container-1`, `demo-plan-container-1`

**Demo 2 - Execution Visualization:**
1. Fixed scene (uses first scene graph from data)
2. Allows selection of instruction only
3. Displays model output, execution plan, AND corresponding video
4. Uses element IDs: `instruction-select-2`, `demo-output-container-2`, `demo-plan-container-2`, `demo-video-container-2`

**Demo data format:**
```json
[
  {
    "scene_graph_name": "scene_graph_1",
    "scene_graph": {},
    "data_items": [
      {
        "instruction": "Task description",
        "model_output": "Model's reasoning and plan",
        "plan": ["step1", "step2", ...],
        "video_id": "video_1"
      }
    ]
  }
]
```

**Key implementation details:**
- Both demos share the same `demoData` loaded from `static/demos/data.json`
- Demo 2 is hardcoded to use `demoData[0]` (first scene graph)
- Videos are loaded from `static/demos/videos/${video_id}.mp4`
- HTML escaping is applied to prevent XSS attacks

## Development Workflow

### Testing Changes

Simply open `index.html` in a browser. No build step required.

**For local testing:**
```bash
# Option 1: Direct file opening
open index.html

# Option 2: Simple HTTP server (recommended for avoiding CORS issues)
python3 -m http.server 8000
# Then navigate to http://localhost:8000
```

### Adding Demo Content

1. Add video files to `static/demos/videos/` with naming pattern `video_N.mp4`
2. Update `static/demos/data.json` with new scene graphs/instructions
3. Ensure `video_id` in JSON matches the video filename (without extension)

### Adding Paper Assets

- Images/figures: `static/images/`
- Videos: `static/videos/`
- PDFs: `static/pdfs/`
- Reference assets directly in `index.html` using relative paths like `static/images/filename.png`

## Content Updates

### Customizing Page Content

The template has TODO comments throughout `index.html` marking places to customize:
- Meta tags (lines 17-54): Update for SEO/social sharing
- Author information (lines 240-251)
- Paper links (lines 256-298): arXiv, GitHub, supplementary materials
- Abstract (lines 422-424)
- Results sections (lines 484-586)
- BibTeX citation (lines 593-610, currently commented out)
- Related works dropdown (lines 199-228)

### Meta Tags & SEO

The page includes comprehensive meta tags for:
- Open Graph (Facebook/LinkedIn)
- Twitter Cards
- Google Scholar citation metadata
- Structured data (Schema.org JSON-LD)

Update the social preview image at `static/images/social_preview.png` (recommended: 1200x630px).

## Dependencies

The project uses CDN-hosted libraries:
- **Bulma CSS**: Main CSS framework
- **jQuery 3.5.1**: DOM manipulation
- **Bulma Carousel/Slider**: Interactive components
- **Font Awesome**: Icons
- **Academicons**: Academic icons (arXiv, etc.)
- **Adobe Document Cloud**: PDF viewer (if used)

All major dependencies are loaded via CDN with fallbacks for offline use.

## Deployment

This site is designed for GitHub Pages:

1. Enable GitHub Pages in repository settings
2. Set source to `master` branch / root directory
3. The `.nojekyll` file prevents Jekyll processing
4. Site will be available at `https://username.github.io/repo-name/`

**Important:** Update all `YOUR_DOMAIN.com` placeholders in meta tags before deploying.

## File Naming Conventions

- Images: Descriptive names like `ahat_planning.jpg`, `main_results_in_public_benchmarks.png`
- Videos: Either descriptive like `ahat_accompanying_video.mp4` or numbered like `video_1.mp4`
- Keep filenames lowercase with underscores or hyphens

## JavaScript Structure

### main script (static/js/index.js)
- More Works dropdown functionality
- BibTeX copy-to-clipboard
- Scroll-to-top button
- Video carousel autoplay
- Bulma component initialization

### Interactive Demo (static/js/interactive-demo.js)
- Async data loading from JSON
- Dropdown population and event handling
- XSS prevention via HTML escaping
- Dynamic video/output rendering
