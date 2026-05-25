PIPELINE 1 — AI Resume Screening


Purpose

This pipeline continuously monitors incoming applicants, extracts resumes, evaluates them using AI, and stores results inside the recruitment database.

Step-by-Step Flow


1. Schedule Trigger

Runs every:

15 minutes

This continuously checks for new candidates.

2. Fetch Pending Candidates

The system pulls candidates from:

SWE Sheet

Contains:

Name
Email
GitHub
Resume
Tech stack
BDM Sheet

Contains:

Name
Email
CV
Experience
City

Only rows with:

status = pending

are processed.

3. Role Assignment

The workflow automatically assigns:

SWE

or

BDM

based on source sheet.

4. Merge Candidate Streams

Both role pipelines are merged into one unified processing pipeline.

5. Validation Check

The workflow verifies:

Candidate email exists
Required data is present

Invalid rows are skipped automatically.

6. Data Normalization

The workflow standardizes fields into a unified structure:

Standard Field	Possible Source Fields
Name	name-1 / name-2
Email	email-1 / email-2
Resume	resume / cv
Tech	tech
Experience	experience

This allows multiple forms/sheets to use different column names.

7. Duplicate Candidate Detection

The workflow checks the central database for existing candidate emails.

If candidate already exists and status is:

screened

the candidate is skipped.

This prevents duplicate processing.

Central Recruitment Database






Main Google Sheet stores:

Column	Description
Name	Candidate name
Email	Candidate email
Resume	Resume URL
Role	Applied role
Github	GitHub profile
Tech	Technology stack
Experience	Work experience
City	Candidate location
Strengths	AI-generated strengths
Concerns	AI-generated concerns
Classification	strong / average / weak
Summary	AI-generated summary
Score	Resume score
Interview_recommendation	yes / no
Status	pending / screened / emailed


8. Resume File Extraction

The workflow:

Extracts Google Drive file ID
Downloads the resume
Extracts text from PDF

Supported format:

PDF
9. AI Resume Evaluation

The workflow sends extracted resume text to:

Groq API

Using model:

llama-3.3-70b-versatile
AI Evaluation Criteria
SWE Candidates

The AI evaluates:

Technical skills
Coding ability
Projects
Tech stack
GitHub profile
BDM Candidates

The AI evaluates:

Sales experience
Communication
Business acumen
Client management
AI Output Structure

The AI returns:

{
  "score": 85,
  "classification": "strong",
  "summary": "Excellent technical background",
  "strengths": [],
  "concerns": [],
  "interview_recommendation": "yes"
}

10. AI Response Cleaning

The workflow:

Extracts JSON from AI response
Cleans malformed outputs
Parses structured data
Handles parsing errors safely

11. Database Update

Candidate record is updated with:

AI score
Classification
Summary
Strengths
Concerns
Interview recommendation
Status = screened

12. Source Sheet Status Update

Original source sheets are updated automatically:

pending → screened

This prevents reprocessing.







PIPELINE 2 — Automated Candidate Communication
Purpose

This pipeline handles:

Interview invitations
Manual review escalation
Rejection emails
Calendar scheduling
Step-by-Step Flow

1. Hourly Trigger

Runs:

Every 1 hour

2. Fetch Screened Candidates

The workflow fetches candidates where:

Status = screened

3. Candidate Classification Routing

Candidates are routed using a Switch node.

Strong Candidates
Actions Performed
Interview Invitation Email

Automated professional email is sent to candidate.

Google Calendar Event Creation

Interview is automatically scheduled:

3 days later
1-hour duration

Candidate is added as attendee.

Status Update

Database updates:

screened → emailed
Average Candidates
Actions Performed



Manual review email is sent to:

hiringmanager@trillesai.com

Contains:



Candidate details
AI score
AI summary

This enables HR intervention.

Weak Candidates
Actions Performed

Automated rejection email is sent professionally.

Database status updated to:

emailed
Technologies Used
Technology	Purpose
n8n	Workflow automation
Google Sheets	Candidate database
Google Drive	Resume storage
Groq API	AI screening
Gmail API	Email automation
Google Calendar API	Interview scheduling
JavaScript	Data transformation
APIs & Integrations
Google Sheets OAuth2

Used for:

Reading candidate sheets
Updating databases
Google Drive OAuth2

Used for:

Downloading resumes
Gmail OAuth2

Used for:

Sending emails
Google Calendar OAuth2

Used for:

Creating interview events
Groq API

Used for:

Resume AI analysis
Required Credentials

Before importing the workflow configure:

Credential	Required
Google Sheets OAuth2	Yes
Google Drive OAuth2	Yes
Gmail OAuth2	Yes
Google Calendar OAuth2	Yes
Groq API Key	Yes




Required Google Sheets
1. SWE Candidate Sheet

Contains SWE applicants.

2. BDM Candidate Sheet

Contains BDM applicants.

3. Central Recruitment Database

Stores:

Candidate information
AI evaluations
Recruitment status
Email Automation

The workflow sends:

Interview Emails

For strong candidates.

Rejection Emails

For weak candidates.

Manual Review Emails

For average candidates.

Candidate Status Lifecycle
pending
   ↓
screened
   ↓
emailed
Automation Benefits
HR Efficiency

Reduces manual screening workload by over 80%.

Faster Hiring

Candidates are screened within minutes.

Consistent Evaluation

AI applies standardized evaluation criteria.

Automated Scheduling

No manual calendar coordination required.

Scalable Recruitment

Can process hundreds of resumes automatically.

Security Notes
Credentials Safety

n8n does NOT export actual OAuth tokens directly in workflow JSON files.

However exported workflows may contain:

Credential references
Credential IDs
Account names

Always remove sensitive references before sharing publicly.

Recommended Improvements
Future Enhancements
ATS Integration
Greenhouse
Lever
Workday
Multi-language Resume Parsing

Support:

Urdu
Arabic
French
AI Interview Question Generation

Generate custom interview questions automatically.

Slack Notifications

Send recruiter alerts instantly.

Candidate Ranking Dashboard

Build live hiring analytics dashboard.

Error Handling Included

The workflow already handles:

Missing emails
Invalid AI JSON
Duplicate candidates
Empty resumes
Missing fields
Performance
Estimated Processing Time
Stage	Approx Time
Resume Download	2–5 sec
PDF Extraction	1–3 sec
AI Evaluation	5–15 sec
Database Update	<1 sec

Average total:

10–25 seconds per candidate
Import Instructions
Step 1

Open n8n.

Step 2

Import the JSON workflow.

Step 3

Reconnect all credentials.

Step 4

Replace:

Google Sheet IDs
Email addresses
Calendar accounts
API keys
Step 5

Activate workflow.

Production Recommendations

For production deployment:

Use environment variables
Enable execution logging
Add retry logic
Add rate limiting
Use separate staging database
Secure API credentials
Add monitoring alerts
Workflow Summary



This workflow provides a fully automated AI-powered hiring pipeline capable of:

Resume collection
Resume parsing
AI screening
Candidate scoring
Interview scheduling
Email communication
Recruitment database management

all with minimal human intervention.

Author

AI Hiring Automation System
Built with n8n + Groq AI + Google Workspace Integration