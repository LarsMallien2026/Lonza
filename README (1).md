# Bob Hands-On Lab: Fix the Supplier Data

**Duration:** ~20 minutes
**Level:** Beginner-friendly, no prior Bob experience required
**Data file:** [`sample_supplier_data_extended.csv`](sample_supplier_data_extended.csv) (30 suppliers)
**Smaller alternative:** [`sample_supplier_data.csv`](sample_supplier_data.csv) (10 suppliers, the first 10 rows of the extended file)

## Goal

Get to know Bob by using its different modes on a small, realistic problem: a supplier master data file full of quality issues.

There is no single "right" answer. The goal is to **explore, experiment and compare**. Try different prompts, see what each mode does well, and find a way of working that suits you.

## The Scenario

You have received an export of supplier master data (10 suppliers). Before it can be loaded into a downstream system, it needs to be cleaned up. The file contains columns such as `supplier_id`, `supplier_name`, `country`, `tax_id`, `contact_email` and `payment_terms`, and not all of the values can be trusted.

## The Three Modes at a Glance

| Mode | Use it to... | Changes your files? |
|------|--------------|---------------------|
| **Ask** | Understand, explain and explore | No |
| **Plan** | Think through an approach and design a solution | Only documents/plans |
| **Agent** | Build, edit, run and test | Yes |

Switch modes with the mode selector in the Bob chat input.

---

## Step 1: Explore the data with Ask mode (~4 min)

Switch to **Ask** mode and get to know the file without changing anything.

Things to try:

- *"Describe the structure of @sample_supplier_data_extended.csv. What columns does it have and what do they mean?"*
- *"What data quality problems can you find in this file?"*
- *"Which rows look like duplicates and why?"*
- *"What format would you expect for a Swiss, German or US tax ID?"*

**Make a note:** What issues did Bob find? Did you spot any that Bob missed (or the other way round)?

---

## Step 2: Plan a solution with Plan mode (~4 min)

Switch to **Plan** mode and design the cleanup before touching any data.

Things to try:

- *"Based on the issues we found, propose a plan to clean @sample_supplier_data_extended.csv. Output the result to a new file."*
- *"Which fixes can be automated safely, and which need a human decision (e.g. missing tax IDs, duplicates)?"*
- *"Compare a Python script vs. a pure manual edit of the CSV. What are the pros and cons?"*

Answer Bob's clarifying questions and challenge the plan. Do you agree with it?

**Make a note:** What did you decide, and why? What should *not* be changed automatically?

---

## Step 3: Build your solution with Agent mode (~5 min)

Switch to **Agent** mode and let Bob implement the plan.

Things to try:

- *"Implement the plan. Keep the original file untouched and write the cleaned data to `supplier_data_cleaned.csv`."*
- *"Also produce a short report listing every change you made and every row that needs manual review."*
- Let Bob run the solution itself and show you the result.

Tip: Review each action Bob proposes before approving it.

---

## Step 4: Inspect your solution (~3 min)

Do not just trust the result, check it.

- Open the original and the cleaned file side by side. What changed?
- Were any values changed that should not have been?
- Were uncertain cases (missing data, possible duplicates) flagged rather than silently "fixed"?
- Switch back to **Ask** mode and try: *"Explain what the script does, step by step"* or *"Are there any edge cases this solution does not handle?"*

**Make a note:** Would you be comfortable loading this file into a production system?

---

## Step 5: Improve it with Bob (~4 min)

Pick one or more ideas, or come up with your own:

- Add validation rules (email format, tax ID format per country, allowed payment terms).
- Add automated tests for the cleaning logic.
- Generate a summary of before vs. after (rows fixed, rows flagged).
- Make the solution reusable for a much larger file (ask Bob to generate a 1,000-row test file first).
- Add documentation or a short usage section to this README.
- Ask Bob to review its own work: *"Review the solution for bugs, risks and improvements."*

Choose the mode that fits each task, and notice when you switch.

---

## Reflection (if time permits)

- Which mode did you find most useful for which step?
- How did the quality of the result change with the quality of your prompt?
- What would you do differently next time?

## Prompting Tips

- Reference files with `@`, e.g. `@sample_supplier_data_extended.csv`.
- Be specific about the expected output and where it should be written.
- Give context: who uses the data and what system will receive it.
- Iterate. Follow-up prompts are normal and encouraged.
- Don't be afraid to ask Bob *why* it made a choice.
