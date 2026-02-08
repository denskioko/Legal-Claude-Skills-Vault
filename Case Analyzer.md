---
name: legal-case-analyzer
description: Analyzes PDF case law reports to produce a structured summary in FILAC format (Facts, Issue, Law, Analysis, Conclusion). Use this skill to quickly understand legal cases from common law jurisdictions, differentiate ratio decidendi from obiter dicta, and extract key legal reasoning.
---

# Legal Case Analyzer

## Overview

This skill analyzes a legal case law report provided as a PDF and generates a concise, structured summary in the FILAC (Facts, Issue, Law, Analysis, Conclusion) format. Its purpose is to accelerate the understanding of legal cases by extracting the most critical information and distinguishing the core legal principles from supplementary remarks.

## Workflow

Follow this workflow to process a case law report. The process is sequential and each step builds on the previous one.

### Step 1: Ingest and Read the Case Law PDF

The user will provide a case law report in PDF format. Your first step is to extract the complete text content from this document.

1.  **Locate the PDF file** provided by the user in the sandbox environment.
2.  Use the `file` tool with the `read` action to extract all text from the PDF. The entire text is required for a comprehensive analysis.

    ```python
    # Example of reading the PDF file
    print(default_api.file(action="read", path="/path/to/case.pdf"))
    ```

### Step 2: Analyze and Structure the Content in FILAC Format

With the full text of the case, your primary task is to read, understand, and parse it into the five components of the FILAC model. Create a new Markdown file to draft your output.

#### 1. Facts

- **Goal:** Identify the relevant facts of the case.
- **What to look for:** The narrative of events that led to the legal dispute. This includes the parties involved, their actions, and the context of the conflict.
- **Instructions:** Summarize the key facts. Include enough background to make the legal issue understandable, but omit procedural history or irrelevant details.

#### 2. Issue

- **Goal:** State the legal question(s) the court was asked to resolve.
- **What to look for:** Phrases like "The issue before the court is..." or questions framed as "Whether...". The issue is the central legal problem.
- **Instructions:** List the issue(s) as clear, concise questions. If there are multiple issues, list them separately.

#### 3. Law

- **Goal:** List the legal rules, statutes, and precedents applied by the court.
- **What to look for:** Citations to statutes (e.g., "Section 5 of the... Act"), regulations, and other court cases (e.g., *Smith v. Jones*).
- **Instructions:** Create a list of the key legal sources the court relied upon. Do not explain the law in this section; simply list the sources.

#### 4. Analysis

- **Goal:** Explain the court's reasoning process in applying the law to the facts to resolve the issue.
- **Instructions:** This is the most critical section. You must carefully summarize the court's chain of reasoning. Within this summary, you MUST differentiate between the *ratio decidendi* and *obiter dicta*.
    - **Ratio Decidendi:** The core legal principle or rule that was essential for the court to reach its decision. It forms the binding precedent for future cases.
    - **Obiter Dicta:** Other comments or observations made by the judge that were not strictly necessary to decide the case. These are persuasive but not binding.
- **Formatting:** Use Markdown to clearly highlight these two elements within your analysis. For example:

    > The court first considered the precedent set in *Alpha v. Beta*. The judge noted that... This reasoning formed the core of the decision. **Ratio Decidendi:** A manufacturer owes a duty of care to the final consumer.
    >
    > The judge also commented on the potential implications for digital products, although this was not central to the case. *Obiter Dicta:* In the future, courts may need to consider whether software is a 'product' for the purposes of liability.

#### 5. Conclusion

- **Goal:** State the final outcome or ruling of the court.
- **What to look for:** Phrases like "The appeal is dismissed," "The judgment is for the plaintiff," or "It is so ordered."
- **Instructions:** Provide a clear and brief statement of the court's final decision.

### Step 3: Generate the Final Markdown Output

Once you have analyzed the content and structured it into the five FILAC sections, compile it into a single, clean Markdown document. The document should have clear headings for each section (e.g., `## Facts`, `## Issue`, etc.).

**Note:** This functionality depends on available tool integrations for Google Drive. If not available, deliver the Markdown file directly.
