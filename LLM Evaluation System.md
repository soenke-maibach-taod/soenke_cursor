# LLM Evaluation System

## System Overview
A system to evaluate LLM outputs against predefined requirements, with automated testing capabilities through GitHub Actions.

## Core Components

### Input Processing
- Multiple input/output requirement pairs
- General output requirements
- Input metadata tracking:
  - Input type (Text, Speech, Image)
  - Input size
  - Additional metadata as needed

### Directory Structure
```
project/
├── data/
│   ├── input/
│ 	├── output/ # Generated outputs (format: original_filenameresponse.ext)
│ 	└── requirements/
│ 		├── specific/ # Per-file requirements (original_filenamerequirements.txt)
│ 		└── general.yml # Global requirements structured by metadata/filetype
├── app/ # Application control interface
└── evaluation/ # LLM evaluation scripts
```


### Evaluation Process
1. Loop through input files
2. For each input:
   - Check for existing output
   - If output exists: Evaluate against requirements (LLM evaluation)
   - If no output: Skip or generate new (configurable)

## Use Case Example
**Project: [[Klüh Voicebot Project Overview]]**
- Input: Audio files
- Output: Audio transcripts (more cost-effective for verification)
- Evaluation: LLM checks if transcripts meet defined requirements

## Technical Notes
- GitHub Actions integration for automated testing
- Flexible app control interface
- Optional output generation