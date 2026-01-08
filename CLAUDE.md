# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

BinfoSkills analyzes bioinformatics job market skills using a three-stage pipeline:
1. **Collection**: Manual collection of job postings into `data/job_posts/*.md`
2. **Extraction**: Python script uses Claude API to extract structured fields from unstructured text
3. **Analysis**: R scripts clean and analyze skill frequency data

## Common Commands

### Python Dependencies
```bash
# Add new dependency
uv add <package-name>

# Run extraction script (requires ANTHROPIC_API_KEY)
python scripts/extract_to_csv.py
```

### R Scripts
```bash
# Clean extracted data
Rscript scripts/clean_data.R

# Analyze skills and generate visualizations
Rscript scripts/analyze_skills.R
```

### Environment Setup
```bash
# Set API key (required for extraction)
export ANTHROPIC_API_KEY='your-key-here'
```

## Architecture

### Data Flow
```
data/job_posts/*.md → extract_to_csv.py → data/processed/jobs.csv → clean_data.R → data/processed/cleaned_jobs.csv → analyze_skills.R → output/ + analysis/
```

### Key Design Decisions

**Markdown for raw data**: Job postings stored as `.md` files preserve structure (headers, bullet lists), making it easier for LLMs to distinguish "Requirements" vs "Nice to have" sections.

**Filename convention**: `{number}_{platform}_{keyword}.md` (e.g., `015_indeed_scrna.md`) encodes metadata for automatic extraction by scripts.

**Semicolon-separated arrays in CSV**: Array fields (required_skills, tools_mentioned, etc.) stored as semicolon-separated strings in CSV for easier editing and LLM processing. R cleaning script explodes these into long format.

**LLM extraction metadata**: Every row includes extraction_model, extracted_at, source_file, source_platform to enable quality assessment and re-extraction if prompts improve.

**Two-stage processing**: Separate extraction (Python/LLM) from cleaning (R) allows iterating on standardization rules without re-running expensive API calls.

### File Organization

- `data/job_posts/`: Raw input data (version controlled, append-only)
- `data/processed/`: Intermediate CSV files (gitignored, regenerated from raw)
- `scripts/`: Processing pipeline scripts
- `output/`: Final deliverables (ranked skills CSV, summary reports)
- `analysis/`: Visualizations (PNG plots)

### Data Schema

**jobs.csv**: Direct output from LLM extraction
- Extracted fields: job_title, company, location, salary, skills arrays, domain, seniority, remote
- Metadata: source_file, source_platform, source_url, extracted_at, extraction_model
- Arrays stored as semicolon-separated strings

**cleaned_jobs.csv**: Long-format after cleaning
- Each skill/tool becomes its own row
- Standardized (lowercase, trimmed, typos fixed)
- Ready for frequency counting and grouping

## Development Notes

**API costs**: Uses Claude 3 Haiku for extraction to minimize costs while maintaining quality. Each job posting ~$0.001-0.003.

**Error handling**: extraction script logs failures but continues processing remaining files. Allows manual review of problematic postings.

**Idempotency**: Scripts can be re-run safely. CSV extraction appends rather than overwrites. R scripts overwrite outputs.
