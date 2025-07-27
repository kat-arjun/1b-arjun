# Challenge 1B - Team ARJUN

This system extracts and prioritizes the most relevant sections from a collection of documents based on a specific persona and their job-to-be-done.

## Features

- PDF text extraction with page number preservation
- Section and subsection identification
- Relevance ranking based on persona and job-to-be-done
- JSON output generation
- Multi-collection document processing
- Persona-based content analysis

## Requirements

- Docker (recommended)
- Python 3.10+ (for non-Docker usage)

## Project Structure

```
Challenge_1b/
├── Collection 1/                    # Travel Planning
│   ├── PDFs/                       # South of France guides (7 documents)
│   ├── challenge1b_input.json      # Input configuration
│   └── challenge1b_output.json     # Analysis results
├── Collection 2/                    # Adobe Acrobat Learning
│   ├── PDFs/                       # Acrobat tutorials (15 documents)
│   ├── challenge1b_input.json      # Input configuration
│   └── challenge1b_output.json     # Analysis results
├── Collection 3/                    # Recipe Collection
│   ├── PDFs/                       # Cooking guides (9 documents)
│   ├── challenge1b_input.json      # Input configuration
│   └── challenge1b_output.json     # Analysis results
├── main.py                          # Main orchestration script
├── pdf_extractor.py                # PDF text extraction
├── section_processor.py            # Section identification
├── relevance_ranker.py             # Relevance ranking
├── output_generator.py             # JSON output generation
├── requirements.txt                 # Python dependencies
├── Dockerfile                       # Docker configuration
└── README.md                        # This file
```

## Collections Overview

### Collection 1: Travel Planning
- **Challenge ID**: round_1b_002
- **Persona**: Travel Planner
- **Task**: Plan a 4-day trip for 10 college friends to South of France
- **Documents**: 7 travel guides

### Collection 2: Adobe Acrobat Learning
- **Challenge ID**: round_1b_003
- **Persona**: HR Professional
- **Task**: Create and manage fillable forms for onboarding and compliance
- **Documents**: 15 Acrobat guides

### Collection 3: Recipe Collection
- **Challenge ID**: round_1b_001
- **Persona**: Food Contractor
- **Task**: Prepare vegetarian buffet-style dinner menu for corporate gathering
- **Documents**: 9 cooking guides

## Building the Docker Image

**Important**: First update the Dockerfile to use Python 3.10 to fix dependency compatibility:

```dockerfile
FROM python:3.10-slim
```

Then build the Docker image:

```bash
docker build -t document-intelligence .
```

## Running the System (Example, how we used in our local machine)

### Option 1: With Docker (Recommended)

#### Collection 1 - Travel Planning
```bash
docker run -v "d:/adobe-final/Challenge_1b/Collection 1:/data/input" -v "d:/adobe-final/Challenge_1b/Collection 1:/data/output" document-intelligence
```

#### Collection 2 - HR Professional Forms
```bash
docker run -v "d:/adobe-final/Challenge_1b/Collection 2:/data/input" -v "d:/adobe-final/Challenge_1b/Collection 2:/data/output" document-intelligence
```

#### Collection 3 - Menu Planning
```bash
docker run -v "d:/adobe-final/Challenge_1b/Collection 3:/data/input" -v "d:/adobe-final/Challenge_1b/Collection 3:/data/output" document-intelligence
```

### Option 2: Without Docker

#### Prerequisites
```bash
pip install -r requirements.txt
```

#### Collection 1 - Travel Planning
```bash
python main.py --input "d:/adobe-final/Challenge_1b/Collection 1/challenge1b_input.json" --output "d:/adobe-final/Challenge_1b/Collection 1/challenge1b_output.json" --debug
```

#### Collection 2 - HR Professional Forms
```bash
python main.py --input "d:/adobe-final/Challenge_1b/Collection 2/challenge1b_input.json" --output "d:/adobe-final/Challenge_1b/Collection 2/challenge1b_output.json" --debug
```

#### Collection 3 - Menu Planning
```bash
python main.py --input "d:/adobe-final/Challenge_1b/Collection 3/challenge1b_input.json" --output "d:/adobe-final/Challenge_1b/Collection 3/challenge1b_output.json" --debug
```

### Custom Input and Output Files

You can specify custom input and output file paths:

```bash
docker run -v /path/to/input/directory:/data/input -v /path/to/output/directory:/data/output document-intelligence --input /data/input/custom_input.json --output /data/output/custom_output.json
```

## Input JSON Format

```json
{
    "challenge_info": {
        "challenge_id": "round_1b_XXX",
        "test_case_name": "specific_test_case",
        "description": "Brief description"
    },
    "documents": [
        {
            "filename": "document1.pdf",
            "title": "Document 1"
        },
        {
            "filename": "document2.pdf",
            "title": "Document 2"
        }
    ],
    "persona": {
        "role": "Travel Planner"
    },
    "job_to_be_done": {
        "task": "Plan a trip of 4 days for a group of 10 college friends."
    }
}
```

## Output Format

The system produces a JSON file with the following structure:

```json
{
    "metadata": {
        "input_documents": ["document1.pdf", "document2.pdf"],
        "persona": "Travel Planner",
        "job_to_be_done": "Plan a trip of 4 days for a group of 10 college friends.",
        "processing_timestamp": "2025-07-24T15:30:45.123456"
    },
    "extracted_sections": [
        {
            "document": "document1.pdf",
            "section_title": "Section Title",
            "importance_rank": 1,
            "page_number": 5
        }
    ],
    "subsection_analysis": [
        {
            "document": "document1.pdf",
            "refined_text": "Refined text from the subsection...",
            "page_number": 5
        }
    ]
}
```

## Key Features

- **Persona-based content analysis**: Tailors extraction to specific user roles
- **Importance ranking**: Ranks sections 1-5 by relevance to the job-to-be-done
- **Multi-collection processing**: Handles different document types and use cases
- **Structured JSON output**: Consistent format with metadata and timestamps
- **Page number preservation**: Maintains source location for all extracted content

## Performance Constraints

- Runs on CPU only
- Model size ≤ 1GB
- Processing time ≤ 60 seconds for document collection (3-5 documents)
- No internet access required during execution

## Technical Implementation

The system is implemented as a modular Python application:

1. **pdf_extractor.py**: Handles PDF text extraction with page preservation
2. **section_processor.py**: Identifies and processes sections and subsections
3. **relevance_ranker.py**: Ranks sections by relevance to the persona and job
4. **output_generator.py**: Generates the final JSON output
5. **main.py**: Orchestrates the entire process

## Troubleshooting

### Docker Build Issues
If you encounter dependency errors during Docker build, ensure the Dockerfile uses `python:3.10-slim` instead of `python:3.9-slim`.

### Path Issues on Windows
Use forward slashes or escape backslashes in paths when running Docker commands on Windows.
```
        
