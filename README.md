# Scientific Resume Screening Demo

A quick demonstration of using AI agents with GitHub Copilot for automated resume screening in biomedical research hiring.

&emsp;&emsp;&emsp;&emsp;Click here ➡️ 
<a href="https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/skilldevai/rescore">
  <img src="https://img.shields.io/static/v1?label=Dev%20Containers&message=Open&color=blue&style=for-the-badge"
       alt="Open in Dev Containers" height="48">    
</a>     

 <br>

## Files Included

```
resume-screener/
├── AGENTS.md                    # Agent definition for Copilot
├── skills/
│   └── resume-scoring.md        # Scoring rubric and instructions
├── sample-resumes/
│   ├── sarah-chen.pdf           # Sample: Highly qualified candidate
│   └── marcus-johnson.pdf       # Sample: Entry-level candidate
├── screen_resumes.py            # Batch processing script
├── generate_pdf_resumes.py      # Script used to create sample PDFs
└── README.md                    # This file
```

## Demo Options

### Option 1: GitHub Copilot Agent Mode 

AI agents with minimal setup

```
User prompt → Copilot reads AGENTS.md → Reads rubric → Extracts PDF → Scores
```

1. Open project in VS Code with GitHub Copilot
2. Type: `@workspace Review and score sample-resumes/sarah-chen.pdf using skills/resume-scoring.md`
3. Watch Copilot apply the rubric and generate evaluation

### Option 2: Python Script 

Showing how to build a real automated pipeline

```
screen_resumes.py → Extract PDF text → Call Claude API → Save evaluation report
```

1. Set API key: `export ANTHROPIC_API_KEY="sk-ant-..."`
2. Run: `python screen_resumes.py`
3. Show the evaluation reports in `evaluations/`

---



### 1.The Structure 
- `AGENTS.md` - defines the agent's task and workflow
- `skills/resume-scoring.md` - contains the transparent scoring rubric
- sample PDFs - to show realistic resume format
- Key point: **Criteria are auditable and adjustable by non-technical stakeholders**

### 2. Runing the Agent (3-5 min)
In GitHub Copilot Chat (Agent mode):

```
@workspace Review and score the resume in sample-resumes/sarah-chen.pdf 
using the scoring rubric in skills/resume-scoring.md
```

Look for the process to:
- Extract text from the PDF
- Parse the resume sections
- Apply each scoring category
- Generate structured output with evidence

### 3. Compare Results 
Run on second resume:

```
@workspace Review and score sample-resumes/marcus-johnson.pdf
```

Highlight:
- Consistent criteria applied to both
- Different scores, clear reasoning
- Audit trail for each decision

### 4. Batch Processing Demo 

Run the Python script to show how this scales:

```bash
python pipeline/screen_resumes.py
```

Extracts text from all PDFs and prepares prompts for LLM processing.

### 5. Discussion Points (2 min)
- **Fairness**: Rubric applied consistently, no implicit bias
- **Transparency**: Hiring committee can see exactly why scores differ
- **Adjustability**: Client can modify weights (more emphasis on publications? Add it!)
- **Scalability**: Run on 100 resumes overnight


## Technical Requirements

```bash
pip install pdfplumber anthropic
```

- **pdfplumber**: Extracts text from PDF resumes
- **anthropic**: Claude API client (optional - script works without it)
- **reportlab**: Only needed if generating sample PDFs

## Python Script Usage

The `screen_resumes.py` script provides a complete end-to-end pipeline:

### With API Key (Fully Automated)

```bash
# Set your API key
export ANTHROPIC_API_KEY="sk-ant-..."

# Process all resumes in sample-resumes/
python screen_resumes.py

# Process a single resume
python screen_resumes.py path/to/resume.pdf

# Process a different directory
python screen_resumes.py --dir applications/
```

**Output:** Markdown evaluation files in `evaluations/` directory with scores, evidence, and recommendations.

### Without API Key (Prompt-Only Mode)

```bash
# Just run without setting API key
python screen_resumes.py
```

**Output:** Prompt files you can paste into claude.ai manually. Useful for demos where you don't want to expose an API key.

### Command Line Options

```
python screen_resumes.py --help

Options:
  --dir, -d      Directory of PDFs to process (default: sample-resumes/)
  --output, -o   Where to save evaluations (default: evaluations/)  
  --rubric, -r   Path to scoring rubric (default: skills/resume-scoring.md)
  --api-key, -k  Anthropic API key (or use ANTHROPIC_API_KEY env var)
```

## Customization Options

1. **Modify `skills/resume-scoring.md`**:
   - Adjust point weights
   - Add position-specific technical skills
   - Include specific research focus areas

2. **Update `AGENTS.md`**:
   - Add mission statement language
   - Include specific ethical guidelines they follow
   - Customize output format for HR system

## Cost Comparison

| Approach | Setup Time | Per-Resume Cost | Accuracy |
|----------|------------|-----------------|----------|
| Manual Review | None | ~$15-30/resume (15-30 min @ senior salary) | Variable |
| Agent + Skills | 2-4 hours | ~$0.05-0.10/resume | Consistent |
| Fine-tuned Model | 40-80 hours | ~$0.01-0.02/resume | High volume |

**Break-even for Agent approach: ~50-100 resumes**

## Common Questions

**Q: What about PDF resumes?**  
A: Most AI tools can parse PDFs. For demo, markdown is cleaner. In production, add a PDF parsing step.

**Q: Can candidates game the system?**  
A: Same risk as keyword-scanning ATS systems. The rubric scores actual achievements, not just keywords.

**Q: What about bias in the rubric itself?**  
A: Great question! The rubric should be reviewed by DEI experts. The transparency makes bias *visible* and *fixable*.

**Q: HIPAA/Privacy concerns?**  
A: For medical research, ensure the AI tool is compliant. Many enterprise offerings (Azure OpenAI, Anthropic API) offer BAAs.
