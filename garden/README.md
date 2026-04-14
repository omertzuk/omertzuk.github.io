# Digital Garden Content

This folder contains all digital garden entries organized by topic. Each entry should be placed in the appropriate topic folder.

## Structure

```
garden/
├── _metadata.yml           # Default settings for all entries
├── README.md              # This file
├── information-theory/    # Topic folder
│   ├── index.qmd         # Topic landing page
│   ├── entry-name.qmd    # Individual entries
│   └── notebook.ipynb    # Or notebook entries
├── complex-systems/       # Topic folder
│   └── index.qmd
├── consciousness/         # Topic folder
│   └── index.qmd
└── ecology/               # Topic folder
    └── index.qmd
```

## Topics

The garden is organized into four main topics:

- **Information Theory**: Entropy, coding, information geometry, predictive coding
- **Complex Systems**: Self-organization, networks, emergence, nonlinear dynamics
- **Consciousness**: Neuroscience, meditation, altered states, theories of mind
- **Ecology**: Dryland systems, eco-hydrology, climate, vegetation patterns

## Creating New Entries

### Markdown/Quarto Entry

Create a new `.qmd` file in the appropriate topic folder with frontmatter:

```yaml
---
title: "Your Entry Title"
description: "A brief description"
date: "YYYY-MM-DD"
categories: [topic-name, growth-stage, other-tags]
---
```

Example: `garden/consciousness/predictive-processing-notes.qmd`

### Jupyter Notebook Entry

Create a new `.ipynb` file in the appropriate topic folder. The first cell should be a markdown cell with frontmatter.

Example: `garden/complex-systems/network-analysis.ipynb`

## Growth Stages

Use these categories to indicate maturity:

- `seedling` 🌱: Fresh ideas, early explorations
- `growing` 🌿: Developing thoughts, work in progress  
- `evergreen` 🌳: Mature, refined ideas

## Content Types

Suggested categories:

- `notebook`: Jupyter notebooks with code and analysis
- `notes`: Reading notes, reflections, summaries
- `experiment`: Technical experiments, demos
- `visualization`: Data visualizations, interactive plots
- `concept`: Conceptual frameworks, mental models
- `meta`: About the garden itself

## Philosophy

Remember: The garden is for **thinking in public**. Content doesn't need to be polished or complete. It should grow and evolve over time.
