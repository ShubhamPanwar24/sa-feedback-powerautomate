# Fixed No-Duplicate GitHub + Power Automate Tool

Fixes included:
1. Submit button is locked while submission is running.
2. Browser stores already submitted Appointment Numbers in localStorage.
3. Each submission includes a Submission ID.
4. Updated Excel template includes Submission ID column.

Most important Power Automate fix:
- Response action must be OUTSIDE Apply to each.
- Use only one Apply to each over rows.
- Inside Apply to each only Add a row into table.
- If Excel rows are inserted but app says failed, your flow is failing after insert. Fix Response action and flow run errors.

If duplicates already exist:
- Open Excel output.
- Use Remove Duplicates on Appointment Number + Submission ID, or Appointment Number + Submitted Time as needed.
