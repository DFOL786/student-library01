# 📚 Student Library + Personal AI + Student Accounts

Student Library is a Streamlit-based AI workspace for students. It combines the original Student Library tools, an evidence-grounded Personal AI career/application assistant, and a free built-in student Login / Sign Up system.

## Login / Sign Up

The whole workspace is protected by a student account screen. New students can create an account with:

- **Name**
- **Gmail** (`@gmail.com`)
- **Address**
- **Password**
- **Confirm password**

Returning students log in with **Gmail + Password**. Passwords are never stored in plain text; the app stores salted **PBKDF2-HMAC-SHA256** hashes. Gmail addresses are unique, so one Gmail cannot create duplicate accounts.

A **My Account** page lets the signed-in student update name/address and change the password. Logout is available from the sidebar.

## Free database

Student accounts use **SQLite**, included with Python and completely free—no paid database subscription or API key is required. The database is created automatically at:

```text
data/student_library.db
```

The table stores account fields only: name, Gmail, address, password hash/salt, account timestamps, and last-login time. Uploaded study documents, resumes, papers, and Personal AI profile files are **not** written into this account database.

> **Deployment note:** SQLite is ideal for local use and a single persistent server. Streamlit Community Cloud can restart/redeploy an app and its local filesystem should not be treated as durable long-term storage. For a production public app where accounts must survive every cloud restart, move the same account table to a persistent hosted database. The included SQLite option itself has zero database cost.

## Modes

1. 📖 **Study Assistant**
2. 💼 **Job Analyzer**
3. 🔬 **Research Assistant**
4. 🎯 **Personal AI**
5. 👤 **My Account**

The app uses Groq or Gemini for AI generation and local FAISS + TF-IDF retrieval for uploaded documents.

## Project structure

```text
student-library/
├── app.py
├── student_auth.py
├── requirements.txt
├── README.md
├── .gitignore
└── data/
    └── .gitkeep
```

Keep `app.py`, `student_auth.py`, `requirements.txt`, and `README.md` in the repository root. The `data` directory is created/used automatically.

## 🎯 Personal AI features

The Personal AI mode was added to make Student Library work like a reusable student application assistant instead of treating every application as a completely new task.

### Dashboard

Shows whether your personal profile and opportunity are ready, plus the number of profile documents and indexed evidence chunks.

### My Profile

Upload your own supporting documents, such as:

- CV / resume
- Certificates
- Project descriptions
- Awards
- Volunteering or leadership evidence
- Experience documents
- Education or skill documents

Supported formats: **PDF, DOCX, PPTX, TXT**.

The app extracts the text, splits it into evidence chunks, and builds a source-aware local FAISS + TF-IDF knowledge base for the current Streamlit session.

### Opportunity

Upload or paste a:

- Job
- Internship
- Scholarship
- Fellowship
- University/program opportunity

The AI extracts explicit eligibility, education, skills, experience, preferred qualifications, required documents, criteria, application questions, and other requirements.

### Match Analysis

Compares opportunity requirements against evidence found in your personal profile and shows:

- Matches
- Gaps / missing evidence
- Unclear requirements
- Eligibility score
- Skills score
- Experience score
- Documents score
- Other score
- Equal-weight readiness estimate
- Retrieved evidence excerpts with source file names

The readiness number is an AI-assisted evidence-match estimate, not a guaranteed probability of acceptance.

### Application Assistant

Ask questions such as:

- Why should I be selected?
- Write an answer about my relevant experience.
- Explain how my projects match this opportunity.
- Draft a scholarship/application response using my evidence.

Answers are grounded in retrieved personal-profile evidence. The latest response can also be refined to be shorter, more formal, more personal, or clearer without adding unsupported facts.

### Tailored Resume

Creates a Markdown resume tailored to the analyzed opportunity using only evidence from your uploaded profile documents. The generated resume can be downloaded as:

- Markdown
- Microsoft Word (.docx)
- PDF

## Existing Student Library features

### 📖 Study Assistant

Upload PDF, DOCX, PPTX, or TXT files and request:

- Document-grounded Q&A
- Quick, standard, or deep summaries
- MCQs
- True/False questions
- Short-answer questions
- Fill-in-the-blank questions
- Flashcards
- Glossaries
- Study plans
- Alternative explanations

### 💼 Job Analyzer

Upload a resume and one or more job descriptions. It provides:

- Technical Skills Match
- Experience Match
- Keyword/ATS Match
- Matching skills
- Missing skills
- Keyword gaps
- Suggested improvements
- Rewritten resume bullets using existing evidence

The three percentages remain separate rather than being combined into one overall score.

### 🔬 Research Assistant

Upload research papers and use:

- Paper Q&A
- Methodology explanations
- Summaries
- APA 7
- MLA 9
- IEEE
- Chicago
- Harvard
- Paper comparison
- Literature-review drafting
- Research-gap analysis
- Citation metadata extraction

The app is instructed not to invent missing citation information.

## Deploy from GitHub to Streamlit Community Cloud

1. Create a GitHub repository and upload `app.py`, `requirements.txt`, and `README.md` to the repository root.
2. Create a new Streamlit Community Cloud app.
3. Set the main file path to:

```text
app.py
```

4. In Streamlit **Secrets**, add at least one provider key:

```toml
GROQ_API_KEY = "your-groq-api-key"
GEMINI_API_KEY = "your-gemini-api-key"
```

You can use only one provider if preferred.

## Privacy and grounding

- Student account fields are stored in the local SQLite account database.
- Passwords are salted and hashed; plain-text passwords are not stored.
- Uploaded files are processed in the current Streamlit session.
- The app does not intentionally save uploaded profile/study documents to the account database.
- Personal AI profile retrieval keeps source names with evidence chunks.
- Prompts explicitly prohibit inventing unsupported personal facts.
- Your selected AI provider receives the extracted context needed to answer the request according to that provider's service terms.
- Do not upload passwords, payment information, private credentials, or other highly sensitive information.

## Important notes

- Scanned/image-only PDFs may require OCR before upload because PDF extraction is text-based.
- Never commit API keys to GitHub.
- FAISS + TF-IDF runs locally in the Streamlit app for retrieval; AI generation uses the selected Groq or Gemini model.
