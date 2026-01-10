# AI Hiring Agent for Small Teams
🔗 Live Demo:[ https://ai-hiring-agent.onrender.com/](url)

⚠️ *Note:* If the link does not open correctly when clicked, please copy and paste it into a new browser tab.

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

<img width="954" height="290" alt="image" src="https://github.com/user-attachments/assets/3855ecca-85ac-4b9b-aab6-3bd1675bde82" />

2. **Job Analyzer Agent**: Parses job requirements and identifies key criteria

<img width="865" height="254" alt="image" src="https://github.com/user-attachments/assets/087d52b7-7809-493d-89cc-3fa88bda415d" />

3. **Evaluator Agent**: Scores candidates based on alignment with job requirements

<img width="750" height="151" alt="image" src="https://github.com/user-attachments/assets/8b859fba-67cd-4d60-896c-b54d630ef04b" />

4. **Question Generator Agent**: Creates targeted interview questions

<img width="713" height="146" alt="image" src="https://github.com/user-attachments/assets/fe3ad5f7-238a-4292-947c-2052571d39a7" />

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
git clone <(https://github.com/shiv4321/ai-hiring-agent)>
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
https://ai-hiring-agent.onrender.com/
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

### Render
1. Create new Web Service
2. Connect repository
3. Build command: `pip install -r requirements.txt`
4. Start command: `uvicorn main:app --host 0.0.0.0 --port $PORT`

## Limitations & Assumptions
- PDF parsing is basic (no OCR for images)
- Handles up to 10 resumes at once (⚠️ Since, the web app is deployed on render's free tier : it has a limit of 512MB RAM so currently try processesing one or two at time.)
- Assumes resumes are in English
- No authentication or user management
- No persistent storage


