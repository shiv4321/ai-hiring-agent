# AI Hiring Agent for Small Teams
Deployed Web App: [https://click here]([url](https://ai-hiring-agent.onrender.com/))
## Overview
An AI-powered hiring assistant that automates candidate evaluation by analyzing resumes against job descriptions. Built with FastAPI backend and vanilla JavaScript frontend.

## Features
- Upload job descriptions and candidate resumes (PDF/text)
- AI-powered resume analysis using LangGraph agents
- Consistent scoring and evaluation
- Knowledge gap identification
- Targeted interview question generation
- Resistance to keyword stuffing

## Architecture

### Agent Design
The system uses a multi-agent workflow orchestrated by LangGraph:

1. **Resume Parser Agent**: Extracts structured information from resumes
2. **Job Analyzer Agent**: Parses job requirements and identifies key criteria
3. **Evaluator Agent**: Scores candidates based on alignment with job requirements
4. **Question Generator Agent**: Creates targeted interview questions

**Scoring Methodology**:
- Experience Match (0-30 points): Years and relevance of experience
- Skills Alignment (0-30 points): Technical and soft skills match
- Education Fit (0-20 points): Educational background relevance
- Overall Fit (0-20 points): Career trajectory and cultural indicators

**Anti-Keyword-Stuffing**:
- Analyzes context and concrete examples, not just keyword presence
- Evaluates depth of experience through project descriptions
- Identifies vague or unsubstantiated claims

### Tech Stack
- **Backend**: FastAPI, LangGraph, Groq API (llama-3.3-70b-versatile)
- **Frontend**: HTML, CSS, JavaScript
- **File Processing**: PyPDF2 for PDF parsing
- **Storage**: In-memory (no database)

## Setup Instructions

### Prerequisites
- Python 3.8+
- pip

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd ai-hiring-agent
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Set environment variable (already included in code):
```bash
export GROQ_API_KEY=your_api_key_here

```

4. Run the application:
```bash
uvicorn main:app --reload
```

5. Open browser:
```
http://localhost:8000
```

## API Documentation

### POST /api/analyze
Analyzes candidate resumes against a job description.

**Request**:
- `job_description`: string (job posting text)
- `resumes`: array of files (PDF or text)

**Response**:
```json
{
  "job_analysis": {
    "title": "string",
    "key_requirements": ["string"],
    "required_skills": ["string"]
  },
  "candidates": [
    {
      "name": "string",
      "email": "string",
      "score": 85,
      "breakdown": {
        "experience": 25,
        "skills": 28,
        "education": 18,
        "overall_fit": 17
      },
      "strengths": ["string"],
      "gaps": ["string"],
      "interview_questions": ["string"],
      "reasoning": "string"
    }
  ]
}
```

## Project Structure
```
ai-hiring-agent/
├── main.py                 # FastAPI application
├── agents.py               # LangGraph agent definitions
├── requirements.txt        # Python dependencies
├── static/
│   └── index.html         # Frontend interface
├── samples/
│   ├── job_description.txt
│   ├── resume1.pdf
│   ├── resume2.pdf
│   └── resume3.txt
└── README.md
```

## Sample Test Cases
Sample job description and resumes are included in the `/samples` directory for immediate testing.

## Design Decisions

1. **In-Memory Storage**: No database needed for this MVP, keeping deployment simple
2. **LangGraph**: Chosen for clear agent orchestration and state management
3. **Groq API**: Free tier with llama-3.3-70b-versatile model for cost efficiency
4. **Synchronous Processing**: Simpler implementation, suitable for small batch sizes
5. **PDF Support**: PyPDF2 for basic PDF text extraction

## Deployment

### Vercel (Recommended)
1. Install Vercel CLI: `npm i -g vercel`
2. Run: `vercel`
3. Follow prompts

### Render
1. Create new Web Service
2. Connect repository
3. Build command: `pip install -r requirements.txt`
4. Start command: `uvicorn main:app --host 0.0.0.0 --port $PORT`

## Limitations & Assumptions
- PDF parsing is basic (no OCR for images)
- Handles up to 10 resumes at once
- Assumes resumes are in English
- No authentication or user management
- No persistent storage

## Time Investment
Approximately 10 hours spent on:
- Agent design and implementation (4h)
- FastAPI backend (3h)
- Frontend and integration (2h)
- Testing and documentation (1h)

## Future Enhancements (Not Implemented)
- Async processing for large batches
- Advanced PDF parsing with OCR
- Multi-language support
- Database persistence
- User authentication
