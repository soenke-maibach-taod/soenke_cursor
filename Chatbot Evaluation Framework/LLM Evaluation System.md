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
│   ├── general_requirements.yml
│   ├── requirements_definition.yml
│   ├── input/
│   │   └── <case_name>/
│   │       ├── <input_file_1>
│   │       ├── <input_file_2>
│   │       └── case_requirements.yml
│   ├── output/
│   │   └── <config_hash>__<timestamp>/
│   │       └── <case_name>/
│   │           ├── <output_file_1>
│   │           └── <output_file_2>
│   └── evaluation_results/
│       └── <config_hash>__<timestamp>/
│           ├── evaluation_run.log
│           └── <case_name>__<requirement_name>.json
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

## Evaluation Checks
- Tool Call TRUE/FALSE

## Possible Factors influencing LLM Responses
All of these should be logged.
Hashing them can be used to set config_ids which in turn can help track changes and experiments in config.
This in turn aids in automating the deployment and improvement of a chatbot solution.

### Model Configuration
- LLM Choice
- Embedding Models

### Query Processing
- Query Optimization
- Prompt Engineering
- Thresholding

### Data Processing & Storage
- Data Cleaning
- Chunking Strategy
- Multi-Indexing
- Indexing Algorithms

### Retrieval Settings
- Retrieval Size
- Retrieval Strategy (Hybrid Search, Reranking etc.)
- Metadata Filtering

## Requirements for Evaluation Data Structure
- multi modal (output can be text, image, audio + combinations)
- default names for files (makes it easier to include them in the prompt)
- iterable
- clear connection between input, output and requirements
- logging of architecture config in naming of directories
- standardized quality requirements with similar names and standardized result schema