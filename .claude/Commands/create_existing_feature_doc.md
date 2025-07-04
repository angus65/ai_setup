🔧 Prompt: Analyze Codebase and Document Existing Functional Features

You are tasked with analyzing an existing codebase and extracting its functionality into a human-readable, structured summary that will later be used to write a new Product Requirements Document (PRD). Follow the steps below carefully and thoroughly.


✅ Objective

Create a file called existing_features.md that summarizes all high-level functions in the application and the business logic behind each function. This file will serve as the foundation for writing a new PRD that:
	•	Re-implements the existing functionality, and
	•	Allows for additional features to be added later.


🔍 Step-by-Step Instructions
	1.	Analyze the Codebase:
	•	Traverse all relevant files (e.g., controllers, services, handlers, business logic layers).
	•	Identify major high-level functions or features.
	•	Group related behavior together logically (e.g., all payment-related logic under “Process Payment”).
	2.	For Each Function:
	•	Create a top-level heading in the markdown file using the format: ### Function Name
	•	Use descriptive, business-relevant naming. Avoid generic method names like handleRequest().
	3.	Document Business Logic as Bullet Points:
	•	Under each function, list out individual pieces of business logic, one per bullet point.
	•	Describe the business purpose of each rule or check in clear, non-technical language.
	•	Maintain logical order based on the execution flow in code.
	•	Each bullet point should describe one business rule or decision.
	•	Include conditions, validations, calculations, or state transitions that affect functionality.
	4.	Be Precise and Unambiguous:
	•	Avoid vague phrases like “does something” or “maybe adds a fee.”
	•	Do not include implementation details like method calls or database statements.
	5.	Format Output as a Markdown File:
	•	The final output must be valid Markdown syntax.
	•	Save the complete result into a file named: existing_features.md


✅ Example of Good Output

Create Invoice
	•	Ensure the customer has an active account before generating an invoice.
	•	Skip invoice generation if there is no outstanding balance.
	•	Apply a 10% late fee if the invoice is more than 30 days overdue.
	•	Round all invoice line-item totals to two decimal places.
	•	Flag the invoice as “Pending Approval” if the total exceeds $10,000.

❌ Examples of Bad Output

Bad:

Calls validateCustomer() and then generateInvoice().
✘ Implementation detail — not business logic.

Bad:

Checks balance and might add a fee?
✘ Vague and ambiguous.

Bad:

Contains logic for creating invoices.
✘ Not broken down into discrete business rules.

⸻

📄 Output Deliverable
	•	A single file named existing_features.md
	•	Organized using markdown headings and bullet points
	•	Clear, unambiguous, and business-focused descriptions of each functional feature
