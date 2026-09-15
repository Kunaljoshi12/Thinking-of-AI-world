# Prompt Engineering — Complete Study Material

> **Learning status:** Completed a Prompt Engineering course  
> **Level:** Beginner to Advanced  
> **Topics covered:** 12 Prompt Engineering Techniques

## Table of Contents

1. [What is Prompt Engineering?](#1-what-is-prompt-engineering)
2. [Zero-shot Prompting](#2-zero-shot-prompting)
3. [Few-shot Prompting](#3-few-shot-prompting)
4. [Structured Prompting](#4-structured-prompting)
5. [Decomposition Prompting](#5-decomposition-prompting)
6. [Chain-of-Thought Prompting](#6-chain-of-thought-prompting)
7. [Tree-of-Thought Prompting](#7-tree-of-thought-prompting)
8. [Verification Prompting](#8-verification-prompting)
9. [Context Injection Prompting](#9-context-injection-prompting)
10. [Self-Critique Prompting](#10-self-critique-prompting)
11. [ReAct Prompting](#11-react-prompting)
12. [RAG Prompting](#12-rag-prompting)
13. [Rubric-Based Evaluation Prompting](#13-rubric-based-evaluation-prompting)
14. [Comparison](#14-comparison-of-all-techniques)
15. [Combined Workflow](#15-combined-prompt-engineering-workflow)
16. [Limitations and Important Concepts](#16-limitations-and-important-concepts)
17. [Practical Exercises](#17-practical-exercises)

---

# 1. What is Prompt Engineering?

## Definition

**Prompt Engineering** is the practice of designing, testing, and improving instructions given to an AI model to produce useful, accurate, relevant, and consistent outputs.

A prompt is the instruction or input provided to an AI model.

### Basic Prompt

```text
Explain Python.
```

### Improved Prompt

```text
You are a Python instructor.

Explain Python to a complete beginner.

Requirements:
- Use simple English.
- Explain what Python is.
- Give 3 practical examples.
- Explain variables and functions.
- Include a small code example.
- End with 5 practice questions.
```

The second prompt provides role, task, audience, requirements, and output expectations.

## Common Prompt Elements

```text
Role
  ↓
Task / Instruction
  ↓
Context
  ↓
Constraints
  ↓
Output Format
  ↓
Quality Criteria
```

A prompt does not always need every element. Use only the elements that improve the task.

---

# 2. Zero-shot Prompting

## Definition

**Zero-shot Prompting** is a technique where an AI model performs a task without receiving task-specific examples in the prompt.

### Example

```text
Classify the following review as Positive, Negative, or Neutral.

Review:
"The product arrived on time, but the packaging was damaged."
```

Possible output:

```text
Neutral
```

No examples were supplied.

## When to Use

- Simple classification.
- Summarization.
- Translation.
- General explanations.
- Brainstorming.
- Basic content generation.

## Advantages

- Easy to write.
- Requires no examples.
- Saves context space.
- Useful for quick experiments.

## Limitations

- Output format may be inconsistent.
- Ambiguous instructions may be misunderstood.
- Specialized tasks may need examples.
- The model may interpret labels differently than expected.

## Template

```text
You are a [ROLE].

Perform the following task:

[TASK]

Requirements:
- [REQUIREMENT 1]
- [REQUIREMENT 2]

Return the result in this format:

[OUTPUT FORMAT]
```

---

# 3. Few-shot Prompting

## Definition

**Few-shot Prompting** provides a small number of examples to demonstrate how the model should perform a task.

## Example

```text
Classify sentiment as Positive, Negative, or Neutral.

Example 1:
Input: "I love this product."
Output: Positive

Example 2:
Input: "The product is terrible."
Output: Negative

Example 3:
Input: "The product is okay."
Output: Neutral

Now classify:
Input: "The product is excellent."
Output:
```

Possible output:

```text
Positive
```

## Types

### One-shot Prompting

One example is provided.

### Few-shot Prompting

Multiple examples are provided.

## When to Use

- Consistent output formatting.
- Domain-specific classification.
- Teaching labels.
- Style imitation.
- Structured extraction.

## Advantages

- Improves consistency.
- Clarifies expected output.
- Helps with specialized tasks.
- Demonstrates formatting patterns.

## Limitations

- Examples consume context space.
- Bad examples teach bad patterns.
- Too many examples increase cost and latency.
- Examples do not guarantee correctness.

## Best Practices

1. Use accurate examples.
2. Keep examples relevant.
3. Use a consistent format.
4. Include edge cases when necessary.
5. Avoid contradictory examples.

---

# 4. Structured Prompting

## Definition

**Structured Prompting** organizes instructions, context, constraints, and output requirements into clearly separated sections.

## Unstructured Prompt

```text
Tell me about Python and explain how it is used,
also give examples and tell me why it is useful.
```

## Structured Prompt

```text
# Role
You are a Python instructor.

# Task
Explain Python programming.

# Audience
Complete beginners.

# Topics
1. What is Python?
2. Features of Python.
3. Variables.
4. Functions.
5. Practical applications.

# Requirements
- Use simple English.
- Include code examples.
- Explain technical terms.
- Avoid unnecessary complexity.

# Output Format
Use Markdown headings and bullet points.
```

## Common Formats

### Section-based

```text
Role:
Task:
Context:
Constraints:
Output Format:
```

### XML-style

```xml
<role>
You are a programming instructor.
</role>

<task>
Explain Python variables.
</task>

<constraints>
Use simple English.
Include one code example.
</constraints>
```

### JSON-style

```json
{
  "role": "Python instructor",
  "task": "Explain variables",
  "audience": "Beginner",
  "constraints": [
    "Use simple English",
    "Include one example"
  ],
  "output_format": "Markdown"
}
```

## Advantages

- Easy to read.
- Easy to modify.
- Reduces ambiguity.
- Useful for reusable templates.
- Works well in applications and APIs.

## Limitations

- Structure does not guarantee accuracy.
- Overly complex prompts may confuse the model.
- The model may still ignore instructions.

## Reusable Template

```text
You are a [ROLE].

Your task is to [TASK].

Context:
[CONTEXT]

Input:
[USER INPUT]

Requirements:
- [REQUIREMENT 1]
- [REQUIREMENT 2]

Output Format:
[EXPECTED FORMAT]

Quality Criteria:
[HOW TO JUDGE THE ANSWER]
```

---

# 5. Decomposition Prompting

## Definition

**Decomposition Prompting** divides a complex task into smaller, manageable subtasks.

## Example

Instead of:

```text
Teach me Python from beginner to advanced.
```

Use:

```text
Create a Python learning plan.

Break the plan into:

1. Python fundamentals
2. Control flow
3. Functions
4. Object-oriented programming
5. File handling
6. APIs
7. Projects

For each stage provide:
- Topics
- Learning objective
- Practical exercise
- Mini project
```

## Types

- **Task decomposition:** Break a large task into smaller tasks.
- **Problem decomposition:** Break a problem into causes or components.
- **Sequential decomposition:** Complete one step before the next.
- **Hierarchical decomposition:** Break major areas into smaller subtasks.
- **Modular decomposition:** Create reusable independent modules.

## Advantages

- Makes complex tasks manageable.
- Improves organization.
- Enables separate evaluation.
- Supports multi-step workflows.

## Limitations

- More steps increase cost and latency.
- Early errors can affect later steps.
- Simple tasks may not need decomposition.
- Excessive decomposition creates unnecessary complexity.

## Template

```text
Break the following problem into smaller tasks:

[PROBLEM]

For each task provide:
- Objective
- Inputs
- Actions
- Expected output
- Dependencies
```

---

# 6. Chain-of-Thought Prompting

## Definition

**Chain-of-Thought (CoT) Prompting** encourages a model to solve a problem through multiple intermediate reasoning steps.

It is useful for:

- Mathematical problems.
- Logical reasoning.
- Multi-step planning.
- Complex analysis.
- Constraint-based decisions.

## Example

```text
A shop gives a 20% discount on a product priced at ₹1,000.
What is the final price?

Explain the calculation and verify the result.
```

Possible answer:

```text
Original price = ₹1,000
Discount = 20% of ₹1,000 = ₹200
Final price = ₹1,000 - ₹200 = ₹800

Answer: ₹800
```

## Modern Best Practice

You do not always need to request a complete internal reasoning transcript. A practical instruction is:

```text
Solve the problem carefully.

Verify the calculations.

Provide a concise explanation of the method
and the final answer.
```

## Advantages

- Helps with multi-step tasks.
- Makes calculations easier to follow.
- Supports educational explanations.
- Encourages intermediate checking.

## Limitations

- More reasoning does not guarantee correctness.
- Longer outputs increase token usage.
- The model may produce plausible but incorrect explanations.
- A reasoning trace is not proof.

---

# 7. Tree-of-Thought Prompting

## Definition

**Tree-of-Thought (ToT) Prompting** explores multiple possible solution paths instead of following only one linear path.

## Diagram

```text
                 Problem
                    |
          ---------------------
          |         |         |
       Path A     Path B     Path C
          |         |         |
       Evaluate  Evaluate  Evaluate
          |         |         |
          -------- Best --------
                    |
                 Answer
```

## Chain-of-Thought vs Tree-of-Thought

| Chain-of-Thought | Tree-of-Thought |
|---|---|
| Follows one reasoning path. | Explores multiple paths. |
| Sequential. | Branching. |
| Good for calculations. | Good for alternatives and strategy. |
| Usually simpler. | Usually more expensive. |

## Example

```text
I want to build an AI application.

Compare:
1. Python
2. JavaScript
3. Java

For each option evaluate:
- Learning difficulty
- AI library support
- Development speed
- Deployment
- Maintainability

Compare the options and recommend the best choice.
```

## When to Use

- Strategic decisions.
- Planning.
- Complex puzzles.
- Comparing alternatives.
- Optimization problems.

## Limitations

- More expensive.
- Requires branch evaluation.
- Incorrect evaluation can select a bad path.
- Not useful for simple tasks.

---

# 8. Verification Prompting

## Definition

**Verification Prompting** asks the model to check, validate, or verify an answer before presenting the final result.

## Example

```text
Calculate:

125 × 24

Verify the result using a second method.
Provide the final answer.
```

Possible answer:

```text
125 × 24 = 3,000

Verification:
125 × (20 + 4)
= 2,500 + 500
= 3,000

Final answer: 3,000
```

## Types

- Calculation verification.
- Logic verification.
- Requirement verification.
- Code verification.
- Source verification.
- Output verification.

## Code Example Prompt

```text
Write a Python function to check whether a number is prime.

After writing the code:
1. Review the logic.
2. Test with 2.
3. Test with 1.
4. Test with 9.
5. Test with 13.
6. Identify edge cases.
7. Provide corrected code.
```

## Advantages

- Helps identify errors.
- Improves reliability.
- Validates requirements.
- Supports quality assurance.

## Limitations

- The model can verify incorrectly.
- It may repeat the same error.
- Self-verification is not independent proof.
- External tools may still be required.

## Best Practice

Use independent validation where possible:

```text
Calculate the total using the formula.

Then verify the result using Python.

If the results differ, investigate the discrepancy.
```

---

# 9. Context Injection Prompting

## Definition

**Context Injection Prompting** supplies relevant information to the model so it can produce a more accurate and domain-specific answer.

Context may include:

- Documents.
- Product details.
- Business rules.
- User preferences.
- Technical specifications.
- Database records.
- Previous conversation information.

## Example

```text
You are a customer support assistant.

Context:
Product: Wireless Mouse
Price: ₹799
Warranty: 1 year
Delivery: 3–5 working days

Question:
Explain the product details to a customer.
```

## Context Injection Template

```text
Role:
You are a [ROLE].

Context:
[RELEVANT INFORMATION]

Task:
[WHAT TO DO]

Constraints:
- Use the supplied context where applicable.
- Do not invent missing details.
- If information is unavailable, say so.

Output Format:
[EXPECTED FORMAT]
```

## Context Injection vs RAG

| Context Injection | RAG |
|---|---|
| Supplies context directly. | Retrieves context from a knowledge source. |
| Can be manually written. | Usually uses automated retrieval. |
| Good for small context. | Good for large document collections. |
| Does not require retrieval infrastructure. | Requires retrieval and document processing. |

RAG is one way to implement context injection.

## Advantages

- Improves relevance.
- Grounds responses in supplied information.
- Supports domain-specific answers.
- Reduces guessing.

## Limitations

- Incorrect context creates incorrect answers.
- Too much context increases cost.
- Conflicting context may confuse the model.
- Sensitive information must be protected.

---

# 10. Self-Critique Prompting

## Definition

**Self-Critique Prompting** asks the model to review its own output, identify weaknesses, and improve it.

## Workflow

```text
Generate
   ↓
Critique
   ↓
Identify Issues
   ↓
Improve
   ↓
Final Output
```

## Example

```text
Write a professional email requesting leave.

After drafting:
1. Review the tone.
2. Check grammar.
3. Check clarity.
4. Identify unnecessary sentences.
5. Improve the email.
6. Provide the final version.
```

## Code Review Example

```text
Write a Python function to calculate factorial.

Then review:
- Is the logic correct?
- Does it handle zero?
- Does it handle negative input?
- Is the code readable?
- Are there edge cases?

Improve the code based on the review.
```

## Self-Critique vs Verification

| Self-Critique | Verification |
|---|---|
| Reviews weaknesses and quality. | Checks correctness or validity. |
| Focuses on improvement. | Focuses on validation. |
| Evaluates clarity and style. | Evaluates calculations, requirements, and claims. |

## Limitations

- The model may miss its own errors.
- Criticism may be superficial.
- It may rewrite correct content unnecessarily.
- Self-critique is not independent evaluation.

---

# 11. ReAct Prompting

> Correct spelling: **ReAct**, not “Re-act”.

## Definition

**ReAct** stands for **Reasoning + Acting**.

It is an approach where an AI model combines reasoning with actions such as:

- Calling tools.
- Searching information.
- Querying databases.
- Executing code.
- Observing tool results.
- Deciding the next action.

## Workflow

```text
User Request
     ↓
Understand Task
     ↓
Decide Next Action
     ↓
Call Tool
     ↓
Observe Result
     ↓
Decide Next Action
     ↓
Final Answer
```

## Example

```text
User:
What is the weather in Mumbai?

Workflow:
1. Understand the request.
2. Call a weather API.
3. Observe the weather result.
4. Prepare the answer.
```

## ReAct Prompt Template

```text
You are an AI assistant with access to tools.

Task:
[USER TASK]

Available tools:
- Search
- Calculator
- Database lookup

Instructions:
1. Understand the task.
2. Decide whether a tool is required.
3. Use the appropriate tool.
4. Review the tool result.
5. Continue until the task is complete.
6. Provide a concise final answer.

Do not claim a tool was used unless it was actually called.
Do not invent tool results.
```

## ReAct vs Chain-of-Thought

| ReAct | Chain-of-Thought |
|---|---|
| Combines reasoning and actions. | Focuses on multi-step reasoning. |
| Can call tools. | Does not inherently require tools. |
| Uses observations. | Uses reasoning steps. |
| Common in AI agents. | Common in reasoning tasks. |

## Applications

- Research assistants.
- Customer support agents.
- Shopping assistants.
- Travel assistants.
- Database assistants.
- Coding agents.
- Automation systems.

## Limitations

- Tool failures can interrupt workflows.
- Incorrect actions can create bad results.
- Requires permissions and safe execution.
- More steps increase latency and cost.

---

# 12. RAG Prompting

## Definition

**RAG** stands for **Retrieval-Augmented Generation**.

RAG retrieves relevant information from an external knowledge source and supplies it to an LLM before generating an answer.

## Why RAG Is Needed

A model may not know:

- Internal company documents.
- Latest private information.
- Product catalogs.
- Company policies.
- Data stored in databases.

## Architecture

```text
User Question
      ↓
Query Processing
      ↓
Retriever
      ↓
Knowledge Base Search
      ↓
Relevant Documents
      ↓
Context Construction
      ↓
LLM / Generator
      ↓
Final Answer
```

## Main Components

1. **Knowledge source:** PDFs, websites, databases, manuals, policies.
2. **Document processing:** Loading, cleaning, chunking, and embedding.
3. **Vector database:** Stores vector representations.
4. **Retriever:** Finds relevant information.
5. **Context:** Retrieved content supplied to the LLM.
6. **Generator:** Produces the final answer.

## Example

### Source Document

```text
Company Leave Policy:

Employees are eligible for 18 annual leave days.
Leave requests must be approved by the manager.
```

### User Question

```text
How many annual leave days are available?
```

### RAG Prompt

```text
You are a company policy assistant.

Use the retrieved context to answer.

Context:
Employees are eligible for 18 annual leave days.

Question:
How many annual leave days are available?

Instructions:
- Answer using the supplied context.
- Do not invent additional policy.
- If information is unavailable, say so.
```

### Expected Answer

```text
Employees are eligible for 18 annual leave days.
```

## RAG Prompt Template

```text
You are a helpful knowledge assistant.

Use the retrieved context to answer the user's question.

Retrieved Context:
[DOCUMENT CONTENT]

User Question:
[QUESTION]

Instructions:
1. Answer based on the retrieved context.
2. Do not invent facts.
3. If context is insufficient, say so.
4. Cite relevant sources when available.
5. Keep the answer clear and concise.

Answer:
```

## RAG vs Fine-tuning

| RAG | Fine-tuning |
|---|---|
| Retrieves information at query time. | Trains the model on additional examples. |
| Good for changing knowledge. | Good for behavior and style adaptation. |
| Can provide source references. | Does not inherently provide citations. |
| Updates knowledge by changing the source. | Often requires another training process. |
| Useful for document Q&A. | Useful for specialized behavior. |

## RAG Evaluation Metrics

### Retrieval

- Recall@K.
- Precision@K.
- Hit Rate.
- MRR.

### Generation

- Answer correctness.
- Faithfulness.
- Relevance.
- Citation accuracy.

### System

- Latency.
- Cost per query.
- Retrieval failure rate.

---

# 13. Rubric-Based Evaluation Prompting

## Definition

**Rubric-Based Evaluation Prompting** asks an AI model to evaluate an answer against explicit criteria and scores.

A rubric is a structured scoring guide that defines what makes an output good or bad.

## Example

```text
Evaluate the following answer.

Question:
What is Python?

Answer:
Python is a programming language used for web development,
automation, data analysis, and AI.

Rubric:
1. Accuracy — 0 to 5
2. Completeness — 0 to 5
3. Clarity — 0 to 5
4. Relevance — 0 to 5

Instructions:
- Give a score for each criterion.
- Explain each score.
- Identify missing information.
- Provide an improved answer.
```

## Types of Rubrics

### Binary

Pass / Fail.

### Numeric

Score from 1 to 5.

### Weighted

Different criteria have different importance.

```text
Accuracy: 50%
Completeness: 25%
Clarity: 15%
Relevance: 10%
```

### Checklist

```text
[ ] Includes a definition
[ ] Includes an example
[ ] Uses simple English
[ ] Avoids unsupported claims
```

## Example for AI Evaluation

```text
Evaluate the chatbot response.

Requirement:
The assistant must answer only from the supplied policy.

Rubric:
1. Policy accuracy — 0 to 5
2. No hallucination — 0 to 5
3. Completeness — 0 to 5
4. Clear communication — 0 to 5

Return:
- Scores
- Reasons
- Violations
- Final verdict
```

## Advantages

- Creates measurable criteria.
- Improves consistency.
- Supports output comparison.
- Useful for automated evaluation.
- Identifies weaknesses.

## Limitations

- Rubrics can be subjective.
- Models may score too generously.
- Poor rubrics produce poor evaluations.
- Scores may not represent real-world correctness.
- Important claims need independent verification.

---

# 14. Comparison of All Techniques

| No. | Technique | Main Purpose | Best Use Case |
|---|---|---|---|
| 1 | Zero-shot | Perform a task without examples. | Simple classification or explanation. |
| 2 | Few-shot | Learn a pattern from examples. | Formatting and classification. |
| 3 | Structured | Organize instructions. | Complex and reusable prompts. |
| 4 | Decomposition | Break tasks into subtasks. | Planning and complex problems. |
| 5 | Chain-of-Thought | Encourage multi-step reasoning. | Calculations and logic. |
| 6 | Tree-of-Thought | Explore multiple paths. | Strategy and alternatives. |
| 7 | Verification | Check results. | Accuracy and validation. |
| 8 | Context Injection | Supply relevant information. | Domain-specific answers. |
| 9 | Self-Critique | Review and improve output. | Writing and code review. |
| 10 | ReAct | Reason and act with tools. | AI agents. |
| 11 | RAG | Retrieve knowledge and generate. | Document Q&A. |
| 12 | Rubric Evaluation | Score output against criteria. | Quality evaluation. |

---

# 15. Combined Prompt Engineering Workflow

Multiple techniques can be combined in real applications.

## Example: AI Study Assistant

```text
User Question
     ↓
Structured Prompt
     ↓
Context / RAG
     ↓
Decomposition
     ↓
Generate Explanation
     ↓
Verification
     ↓
Self-Critique
     ↓
Rubric Evaluation
     ↓
Final Answer
```

## Combined Prompt

```text
# Role
You are an AI learning assistant.

# Task
Explain Generative AI to a beginner.

# Context
Use the supplied study material.

# Instructions
1. Break the topic into smaller sections.
2. Explain each section clearly.
3. Give practical examples.
4. Avoid unsupported claims.
5. Verify important facts.
6. Review the answer for clarity and completeness.

# Rubric
Evaluate:
- Accuracy
- Completeness
- Clarity
- Practical examples

# Output Format
1. Definition
2. How it works
3. Examples
4. Advantages
5. Limitations
6. Summary
```

**Important:** Do not combine every technique by default. Start with the simplest technique that solves the task and add complexity only when it provides a measurable benefit.

---

# 16. Limitations and Important Concepts

## Prompt Engineering Is Not Model Training

Prompt engineering changes instructions but does not change model weights.

Fine-tuning trains a model on additional examples and changes its parameters.

## Prompt Engineering Does Not Guarantee Correctness

Even a strong prompt can produce incorrect results because of:

- Model limitations.
- Missing context.
- Incorrect documents.
- Ambiguous instructions.
- Tool failures.
- Outdated information.
- Incorrect reasoning.

## More Instructions Are Not Always Better

Bad prompt:

```text
Explain Python in extreme detail with everything,
all concepts, all examples, all use cases, and all advanced
topics without missing anything.
```

Better prompt:

```text
Explain Python fundamentals to a beginner.

Cover:
1. Variables
2. Data types
3. Conditions
4. Loops
5. Functions

Use simple English and one example per topic.
```

## Context Window

A model can process only a limited amount of information in one request.

Large prompts may:

- Increase cost.
- Increase latency.
- Reduce relevance.
- Exceed model limits.

## Hallucination

A hallucination occurs when an AI generates unsupported, fabricated, or incorrect information.

Useful mitigation techniques include:

- Context injection.
- RAG.
- Verification.
- Source checking.
- Rubric evaluation.

These reduce risk but do not eliminate hallucinations.

## Prompt Injection

Prompt injection is an attempt to manipulate an AI system's instructions through untrusted input.

Security practices:

- Separate trusted instructions from untrusted data.
- Treat retrieved documents as untrusted content.
- Apply access control.
- Avoid exposing secrets.
- Validate tool actions.
- Use least-privilege permissions.
- Test injection scenarios.

## Prompt Templates

A prompt template is a reusable prompt with placeholders.

```text
You are a [ROLE].

Explain [TOPIC] to [AUDIENCE].

Requirements:
- [REQUIREMENT 1]
- [REQUIREMENT 2]

Output Format:
[FORMAT]
```

---

# 17. Practical Exercises

## Exercise 1: Zero-shot

Create a prompt that classifies customer feedback as Positive, Negative, or Neutral.

Input:

```text
"The product arrived late."
```

## Exercise 2: Few-shot

Create three examples that classify text into:

- Bug.
- Feature Request.
- Question.

Then classify:

```text
"Please add dark mode to the application."
```

## Exercise 3: Structured Prompting

Create a structured prompt to explain Python loops to beginners.

Include:

- Role.
- Task.
- Context.
- Requirements.
- Output format.

## Exercise 4: Decomposition

Create a step-by-step prompt to build a portfolio website.

Stages:

1. Requirements.
2. Design.
3. Development.
4. Testing.
5. Deployment.

## Exercise 5: Calculation and Verification

```text
A product costs ₹2,500.
A discount of 12% is applied.
GST of 18% is applied after the discount.

Calculate the final amount.
Show formulas and verify the result.
```

## Exercise 6: Tree-of-Thought

Compare:

- Python.
- JavaScript.
- No-code platform.

Evaluate trade-offs and recommend one.

## Exercise 7: Code Verification

Ask an AI to generate a Python function, review it, test edge cases, and correct errors.

## Exercise 8: Context Injection

Use this context:

```text
Company:
ABC Technologies

Working hours:
9 AM to 6 PM

Work from home:
2 days per week with manager approval
```

Question:

```text
Can an employee work from home 3 days per week?
```

## Exercise 9: Self-Critique

Write an explanation of AI Agents and ask the model to review accuracy, missing information, and clarity.

## Exercise 10: ReAct

Design a prompt for an assistant that:

1. Receives a question.
2. Searches a knowledge base.
3. Uses a calculator if needed.
4. Generates an answer.

## Exercise 11: RAG

Design a RAG chatbot for a company policy document.

Include:

- Document loading.
- Chunking.
- Embeddings.
- Vector database.
- Retrieval.
- LLM generation.

## Exercise 12: Rubric Evaluation

Create a rubric to evaluate an AI answer based on:

- Accuracy.
- Completeness.
- Clarity.
- Relevance.
- Hallucination.

Score each from 1 to 5.

---

# Key Takeaways

## Beginner Techniques

1. Zero-shot Prompting.
2. Few-shot Prompting.
3. Structured Prompting.

## Problem-Solving Techniques

4. Decomposition Prompting.
5. Chain-of-Thought Prompting.
6. Tree-of-Thought Prompting.

## Quality Techniques

7. Verification Prompting.
8. Self-Critique Prompting.
9. Rubric-Based Evaluation Prompting.

## Context and Agent Techniques

10. Context Injection Prompting.
11. ReAct Prompting.
12. RAG Prompting.

## Recommended Learning Progression

```text
Zero-shot
    ↓
Few-shot
    ↓
Structured Prompting
    ↓
Decomposition
    ↓
Verification
    ↓
Context Injection
    ↓
RAG
    ↓
ReAct
    ↓
Rubric-Based Evaluation
    ↓
Advanced AI Applications
```

## Final Summary

Prompt Engineering is the foundation of effective interaction with AI models.

The most important skills are:

- Writing clear instructions.
- Providing relevant context.
- Breaking down complex problems.
- Selecting appropriate techniques.
- Checking outputs.
- Evaluating quality.
- Understanding model limitations.
- Designing safe and reliable AI workflows.
