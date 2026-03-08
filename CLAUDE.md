# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Educational materials for a digital humanities course on Social Network Analysis (SNA) of movie scripts. The workflow extracts character networks from screenplays (sourced from IMSDb), then visualizes them as interactive HTML dashboards.

**Language:** All notebooks, prompts, and documentation are written in Polish.

## Repository Structure

```
notebooks/
  analiza_sieciowa_scenariusza.ipynb         # Student version — prompts only, no code
  analiza_sieciowa_scenariusza_prowadzacy.ipynb  # Instructor version — full reference implementation

prompts/
  prompt_film_sna_visualization.md           # Prompt for generating HTML SNA dashboards from GraphML

sna_dashboards/<film>/
  sna_<film>.ipynb       # Completed notebook run for a specific film
  krawedzie.csv          # Edge list: character A | character B | co-occurrence count
  graf_postaci.graphml   # Full network in GraphML format (nodes=characters, edges=co-occurrences, weight attribute)
  <film>.html            # Self-contained interactive SNA dashboard
```

## Workflow

1. **Data extraction** (`notebooks/analiza_sieciowa_scenariusza.ipynb` or the `_prowadzacy` version):
   - Run in Google Colab; set `adres_scenariusza` URL (IMSDb) as the only parameter
   - Pipeline: fetch page → extract script text → detect scenes → detect characters → build co-occurrence pairs → export `krawedzie.csv` + `graf_postaci.graphml`

2. **Dashboard generation** (`prompts/prompt_film_sna_visualization.md`):
   - Use the prompt as a system message in Claude/GPT-4o/Gemini
   - Attach the `.graphml` file and request visualization
   - Output: a single self-contained HTML file with D3.js v7 interactive graph, SNA metrics (degree, betweenness, closeness, PageRank, clustering), community detection, and humanistic interpretation

## Key Data Formats

- **GraphML**: nodes have character names as labels; edges have a `weight` attribute (scene co-occurrence count)
- **krawedzie.csv**: three columns — source character, target character, weight

## Dashboard Technical Constraints

The generated HTML dashboards must be:
- Single self-contained file (inline CSS, JS, data)
- D3.js v7 from cdnjs; Google Fonts via `@import`; no external API calls
- No React/Vue/localStorage; no generic Inter/Roboto fonts
- Film-specific aesthetics: palette, typography, and background texture derived from the film's visual identity
