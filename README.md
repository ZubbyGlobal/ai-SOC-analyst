       AI-Powered SOC Analyst: Automated Attack to Verdict Pipeline

An end-to-end security operations workflow that removes manual triage from the first pass of an investigation. A simulated attack becomes a structured alert, and an AI agent returns a classification, severity score, and rating with zero manual intervention.

       The problem

Most SOC analysts don't struggle because they don't know what to do. They struggle because they have too much to do the same alerts, the same investigations, the same first-pass analysis, repeated hundreds of times a shift. That repetition is where analyst fatigue and missed detections come from.

This project automates that first pass, so a human analyst's time goes to judgment calls, not repetitive triage.

What it does

1. An attack is launched from an attacker VM against a target server VM.
2. A Python script captures the resulting activity on the selected interface(eth0) and parses the raw log data and converts it into a structured alert (JSON).
3. The alert is sent automatically to Airia AI, an AI-based triage agent.
4. Airia AI returns a verdict: what the alert is, a severity score, and a rating, no manual review required to produce that first assessment.

    Architecture



 	Tools & stack

• Virtualization: [Hyper-V]
• Attack simulation: [custom scripts (ping nonstop)]
• Automation : Python [‘os’, `json`, `csv’]
• AI triage: Airia AI agent, called via [API – key / API - URL]
• Alert schema: [ timestamp, source IP, event type, raw log excerpt]

   My role vs. the AI's role

To be transparent about where the engineering work is: I designed and built the attack simulation, the detection setup, the Python pipeline that normalizes raw logs into a structured alert schema, and the integration/prompt design that lets Airia AI produce a consistent, usable verdict. Airia AI performs the triage reasoning on the alert I hand it the pipeline and the data contract around it are mine.

       Walkthrough

1. Attack launched from attacker VM — `[screenshot]`
2. Alert payload built by the Python script — `[screenshot/code snippet]`
3. Airia AI's returned verdict — ![screenshots/'Airia AI's returned verdict.png'](https://github.com/ZubbyGlobal/ai-SOC-analyst/blob/38efa846cdbfae0eeb1fa28ed42adcf00c399a2a/screenshot/Airia-AI's-returned-verdict.png)

       Sample output

```json
{
  "alert_id": "example-001",
  "source": "server-vm",
  "event_type": "[e.g. suspicious process execution]",
  "ai_verdict": {
    "classification": "[e.g. Likely malicious - lateral movement attempt]",
    "severity_score": 8.2,
    "rating": "High",
    "summary": "[1-2 sentence AI explanation]"
  }
}
```

What I'd improve next

- Feed the agent historical alert data to reduce false-positive rate over time
- Add a human-in-the-loop escalation path for high-severity verdicts
- Expand attack coverage beyond [current scope] to test the agent against a wider range of techniques
- Add response-time metrics comparing automated triage vs. manual baseline

   Run it yourself

```bash
# clone the repo
git clone [your-repo-url]
cd [repo-name]

# install dependencies
pip install -r requirements.txt

# configure your Airia AI credentials
cp .env.example .env

# run the pipeline
python main.py
```

---

Built as a hands on project to explore how AI agents can reduce analyst workload in the SOC without removing human judgment from the loop. Feedback welcome.

