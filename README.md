# AI Email Secretary

A generative-AI pipeline that triages an executive's inbox using the **Yesterbox** method: it classifies the previous day's emails, summarizes the ones that matter, drafts first replies, and then grades its own output with an LLM-as-a-judge.

Built as the mid-term project for the Johns Hopkins University *Applied Generative AI* program (AGAI, Week 9).

## The scenario

Alex Carter is a Senior Manager in Software Development at a fictional IT services company, Orion Tech Solutions. A high-profile cloud-security project means his inbox is a mix of escalations, approvals, deadlines, and noise. The goal is a system that tells him what to do first each morning.

## What it does

| Step | Output |
|------|--------|
| **Yesterbox filter** | Keeps only emails received the previous day (51 of 60 in the sample dataset) |
| **Classification** | Each email is placed in one of six categories (Urgent & High-Priority, Deadline-Driven, Routine Updates, Non-Urgent Informational, Personal & Social, Spam/Unimportant) |
| **1A. Executive dashboard** | Counts per category plus a short AI summary of the inbox |
| **1B. Urgent emails** | Subject, date, sender, summary and recommended next step for each urgent email |
| **1C. Deadline-driven emails** | Same format for time-sensitive emails, excluding ones already covered in 1B |
| **2. Draft replies** | A first-response draft for each of the 30 critical emails (urgent + deadline-driven) |
| **3. Evaluation** | An LLM judge scores outputs 1-5 on Relevance, Clarity and Actionability, with strengths and improvements |

Result on the sample inbox: 23 urgent, 12 routine, 7 deadline-driven, 5 spam, 3 non-urgent informational, 1 personal.

## Evaluation results

The LLM judge gave Relevance **5/5**, Clarity **4/5** and Actionability **5/5** in the runs shown in the notebook. The consistent critique was that summaries could be more concise and draft replies more uniformly formatted.

## Tech stack

Python, OpenAI API (`gpt-4o-mini`, temperature 0), pandas, tqdm. Prompt engineering with separate system and user prompts for each task; no fine-tuning.

## Getting started

1. Clone the repo and install dependencies: `pip install -r requirements.txt`
2. Copy `config.example.json` to `config.json` and add your OpenAI API key. This file is git-ignored.
3. Provide the email dataset (see below) at the path used in the notebook.
4. Open `ai_email_secretary.ipynb` and run the cells in order.

## Data

The sample inbox (`Alex_emails_march_04.csv`, 60 synthetic emails with columns `email_id`, `date_received`, `sender`, `subject`, `body`, `main_recipient`) was provided as part of the course and is **not included** in this repository. To run the notebook, supply any CSV with the same columns; "today" is set to 4 March 2025 in the filtering step.

## Known limitations

- The executive dashboard summary in the saved run states "Total Number of Emails from Yesterday: 0" even though the verified count of 51 was passed in; the model misread the field. This is a good example of why numeric facts should be computed in code and only *formatted* by the LLM.
- The judge and the generator are the same model (`gpt-4o-mini`), so scores are likely optimistic.
- Evaluation is over a single run on a small synthetic dataset.

## Ideas for improvement

- Tone-adjusted replies based on each sender's past messages.
- Standardized, more concise output formatting across all tasks.
- A stronger, separate judge model and a human-labeled sample to check the judge.

## License

MIT
