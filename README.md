# GitHub Pages + Power Automate SA Feedback Tool

## Final architecture
1. GitHub Pages hosts a static app.
2. `pending_tasks.json` is updated daily in the GitHub repository.
3. RM / SA Owner opens the GitHub Pages link, selects name, fills feedback.
4. The app sends the feedback JSON to a Power Automate HTTP trigger URL.
5. Power Automate writes each feedback row into an Excel table (`tblFeedback`).

## Files
- `index.html` - GitHub Pages frontend app
- `pending_tasks.json` - daily pending tasks file
- `config.json` - paste Power Automate HTTP POST URL here
- `PowerAutomate_Feedback_Output_Template.xlsx` - Excel output template with table `tblFeedback`
- `power_automate_sample_payload.json` - sample body for trigger/schema
- `power_automate_parse_json_schema.json` - Parse JSON schema

## GitHub daily update
Every day replace only:

```text
pending_tasks.json
```

Keep the same GitHub Pages app URL.

## Power Automate flow summary
Create an Automated/Instant cloud flow with trigger:

```text
When an HTTP request is received
```

Then:
1. Add Compose action with expression:

```text
json(triggerBody())
```

2. Add Parse JSON action:
   - Content: output of Compose
   - Schema: use `power_automate_parse_json_schema.json`
3. Add Apply to each: `rows`
4. Inside Apply to each, add Excel Online (Business) action: `Add a row into a table`
   - File: your uploaded `PowerAutomate_Feedback_Output_Template.xlsx`
   - Table: `tblFeedback`
5. Add Response action with status code 200 and optional body:

```json
{"ok":true}
```

## Important note about HTTP trigger and browser calls
This app sends the payload as `text/plain;charset=UTF-8` to avoid browser CORS preflight issues. That is why the flow should use `json(triggerBody())` before Parse JSON.
