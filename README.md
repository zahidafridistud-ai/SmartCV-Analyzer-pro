# SmartCV-Analyzer-pro
# ATS Resume Checker

A Streamlit app that scores a resume the way an Applicant Tracking System (ATS) would and suggests concrete improvements. Analysis is powered by a Google Gemini Flash model.

## Features

- Upload a resume as **PDF, DOCX or TXT** (max 5 MB)
- Optional **job description** field for a role-specific keyword match
- Overall **ATS score (0-100)** plus five category scores:
  | Category | Weight |
  |---|---|
  | Formatting & ATS parsing | 20% |
  | Keywords & relevance | 25% |
  | Impact & achievements | 25% |
  | Clarity & language | 15% |
  | Completeness | 15% |
- Strengths, weaknesses, missing keywords, prioritized suggestions and example bullet rewrites
- "What an ATS sees" view of the extracted text, so you can spot parsing problems

The model scores each category; the overall score is calculated in code from the weights above, so it is consistent.

> The score is an AI-generated estimate, not the output of a real ATS. Different employers use different systems. Treat it as guidance.

## Project structure

```
app.py             # Streamlit app
requirements.txt   # Python dependencies
README.md          # This file
```

## Run locally

1. Install Python 3.10 or newer.
2. Get a free Gemini API key at <https://aistudio.google.com/apikey>.
3. Install and run:

   ```bash
   pip install -r requirements.txt
   streamlit run app.py
   ```

4. Paste your API key into the sidebar, upload a resume and click **Analyze resume**.

To avoid pasting the key each time, either set an environment variable:

```bash
export GEMINI_API_KEY="your-key"        # macOS / Linux
setx GEMINI_API_KEY "your-key"          # Windows (restart the terminal afterwards)
```

or create `.streamlit/secrets.toml` next to `app.py` (never commit this file):

```toml
GEMINI_API_KEY = "your-key"
```

## Configuration

| Setting | Where | Default |
|---|---|---|
| `GEMINI_API_KEY` | Streamlit secrets or environment variable | none (asked in sidebar) |
| `GEMINI_MODEL` | Streamlit secrets or environment variable, or the sidebar field | `gemini-3.8-flash` |

Gemini model names change over time. If you see a "model not found" error, check the current Flash model name in the [Gemini models documentation](https://ai.google.dev/gemini-api/docs/models) and set `GEMINI_MODEL` (or edit `DEFAULT_MODEL` in `app.py`).

## Deploy on Streamlit Community Cloud

1. Push this folder to a GitHub repository (with `app.py` and `requirements.txt` at the top level).
2. Go to <https://share.streamlit.io> and sign in with GitHub.
3. Click **Create app**, choose your repository, branch `main`, and main file `app.py`.
4. Open **Advanced settings > Secrets** and add:

   ```toml
   GEMINI_API_KEY = "your-key"
   ```

5. Click **Deploy**. The first build takes a few minutes.

Users of your public app will use your API key and quota. If you do not want that, leave the secret out and every user will enter their own key in the sidebar.

## Privacy

Resume text is sent to Google's Gemini API for analysis. The app does not save uploaded files or results. Do not upload documents you are not comfortable sending to a third-party AI service.

## Limitations

- Scanned or image-only resumes have no extractable text; export a text-based PDF or DOCX instead.
- Only the first 20,000 characters of a resume and 8,000 characters of a job description are analyzed.
- Layout problems (columns, tables, graphics) can only be inferred from the extracted text, not seen directly.

## Troubleshooting

| Problem | Fix |
|---|---|
| "Gemini rejected the API key" | Re-copy the key from Google AI Studio; make sure it has no extra spaces |
| "Rate limit or quota reached" | Wait a minute, or check your quota in Google AI Studio |
| "Model name was not found" | Update `GEMINI_MODEL` to a current Flash model |
| "Very little text could be extracted" | Your file is probably an image or scan; export a text-based PDF/DOCX |
| Deployment fails on dependencies | Confirm `requirements.txt` is in the repository root |
