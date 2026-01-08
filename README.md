# BinfoSkills

A project for analyzing bioinformatics job market skills using LLM-powered extraction and statistical analysis.

## Project Overview

This project helps identify the most in-demand skills in the bioinformatics job market by:
1. Collecting job postings from various platforms
2. Using Claude AI to extract structured data from unstructured job descriptions
3. Analyzing skill frequencies and trends with R
4. Generating actionable insights for career development and education

## Project Structure

```
BinfoSkills/
│
├── README.md
│
├── data/
│   │
│   ├── job_posts/
│   │   # Store original job posting text exactly as copied from websites
│   │   # One .md file per job posting (e.g., 001_linkedin_seurat.md)
│   │   # FORMAT: Markdown — preserves structure (headers, lists)
│   │   # NAMING: {number}_{platform}_{keyword}.md (e.g., 015_indeed_scrna.md)
│   │   # HEADER: First line should be source URL: <!-- url: https://linkedin.com/jobs/... -->
│   │
│   └── processed/
│       # Store LLM-extracted and cleaned data ready for analysis
│       # jobs.csv: LLM-extracted fields + extraction metadata
│       # cleaned_jobs.csv: standardized, long-format for analysis
│
├── scripts/
│   │
│   ├── extract_to_csv.py
│   │   # Convert unstructured job text → structured CSV using Claude API
│   │
│   ├── clean_data.R
│   │   # Standardize extracted data for accurate counting
│   │
│   └── analyze_skills.R
│       # Count frequencies, rank skills, generate plots
│
├── output/
│   # Store final deliverables: skills_ranked.csv, summary_report.md
│
└── analysis/
    # Store visualizations: skill_frequency.png, skills_by_domain.png
```

## Workflow

```
[Copy job posts]          [LLM extracts]                [R cleans]         [R analyzes]
      ↓                         ↓                            ↓                   ↓
data/job_posts/*.md  →  extract_to_csv.py  →  processed/jobs.csv  →  clean_data.R  →  analyze_skills.R
                                                                          ↓                   ↓
                                                              processed/cleaned_jobs.csv    output/skills_ranked.csv
                                                                                            analysis/skill_frequency.png
```

## Setup

### Prerequisites
- Python 3.8+
- R 4.0+
- Anthropic API key
- uv (Python package manager)

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd BinfoSkills
```

2. Install Python dependencies:
```bash
uv add anthropic
```

3. Install R dependencies:
```R
install.packages(c("tidyverse", "ggplot2"))
```

4. Set up your Anthropic API key:
```bash
export ANTHROPIC_API_KEY='your-api-key-here'
```

## Usage

### 1. Collect Job Postings

Copy job postings into `data/job_posts/` as markdown files:
- Filename format: `{number}_{platform}_{keyword}.md`
- Example: `001_linkedin_seurat.md`, `002_indeed_scrna.md`
- Add source URL in first line: `<!-- url: https://... -->`

### 2. Extract Data (Coming Soon)

```bash
python scripts/extract_to_csv.py
```

### 3. Clean Data (Coming Soon)

```R
Rscript scripts/clean_data.R
```

### 4. Analyze Skills (Coming Soon)

```R
Rscript scripts/analyze_skills.R
```

## Data Schema

### Extracted Fields (jobs.csv)

- **job_title**: Position title
- **company**: Hiring organization
- **city, country**: Location
- **salary_range, salary_currency**: Compensation info
- **required_skills**: Must-have skills (semicolon-separated)
- **preferred_skills**: Nice-to-have skills (semicolon-separated)
- **tools_mentioned**: Software/tools (semicolon-separated)
- **analysis_types**: Types of analysis (semicolon-separated)
- **domain**: Research area (oncology, immunology, neuroscience, pharma, agricultural, general)
- **seniority_level**: Job level (junior, mid, senior, lead, principal)
- **remote**: Work arrangement (full, hybrid, onsite)

### Metadata Fields

- **source_file**: Original filename
- **source_platform**: Job board (linkedin, indeed, nature, upwork)
- **source_url**: Original posting URL
- **extracted_at**: Extraction timestamp
- **extraction_model**: Claude model used

## License

See LICENSE file for details.