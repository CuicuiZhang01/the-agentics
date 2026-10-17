# CrewAI Build Report

**Name:** Cuicui Zhang

**Topic I used:** how small retail businesses can use AI to forecast inventory demand

## 1. What I changed

I changed the Technical Writer agent's `backstory` in `crewai-kit/main.py`. The original instruction was: "You turn dense technical material into clear, jargon-free prose." My replacement asks the writer to explain technology to a curious 10-year-old, use simple words and short sentences, include familiar examples such as a toy shop, and explain technical terms.

I chose this change to explore how the intended audience affects an agent's output. Inventory forecasting can sound abstract, but deciding how many toys a shop should order makes the problem easier to picture. I kept the same topic, three agents, task descriptions, and sequential process for both runs. I did not add packages or tools. The intended change was a more accessible explanation rather than a different research topic. The changed script is included as `main.py` in this submission.

## 2. How CrewAI works

This program organizes work using agents, tasks, and a crew. An agent describes a role through its `role`, `goal`, and `backstory`. A task describes the work, its expected output, and the responsible agent. The crew combines these agents and tasks into a workflow.

The Research Analyst first generates five key facts and two open questions. The Technical Writer uses the research brief to produce an approximately 250-word explanation. The Editor then tightens the explanation and marks claims needing sources. With `Process.sequential`, later tasks receive context from earlier work. The three agents use the same configured language model; different instructions define their responsibilities.

The command-line topic is passed to `crew.kickoff(inputs={"topic": topic})`, supplying the topic for the prompts. The script prints the final result and saves it to `output/explainer.md`.

Changing the writer's backstory changes its instructions during the writing stage. However, its goal still refers to a "smart reader outside the field," and the editor has no child-specific instructions. These remaining instructions may help explain why the final answer only partly adopted the intended audience. A backstory influences behavior but does not enforce a strict output rule.

## 3. Evidence

Both `run1_original.txt` and `run2_changed.txt` show the same command-line topic. The final articles are saved as `explainer_run1.md` and `explainer_run2.md`.

The original article opens with "Small retailers can improve inventory planning by using AI to forecast demand." It discusses POS systems, e-commerce platforms, SaaS subscriptions, forecast-error percentages, and gross margins. Its four paragraphs read like a business overview for adult readers.

The changed article is titled "Explainer – AI-Driven Demand Forecasting for a Tiny Toy Shop." It begins with a concrete problem: too few toys disappoint customers, while too many leave cash tied up in unsold stock. It organizes the explanation into five numbered steps, including gathering sales data and learning demand patterns. The toy-shop setting directly reflects the new backstory.

What surprised me was that the changed output remained technical. It used "CSV," "SKU," "time-series algorithm," and "safety-stock levels" without clearly explaining all of them. The change influenced the example and presentation, but did not fully produce an explanation suitable for a 10-year-old. In this comparison, I recorded that limitation rather than treating the change as a complete success. A future revision could align the writer's goal and editor's task with the younger audience; that revision is not part of these submitted runs.

The articles also contain different factual claims. With only one run per version, I cannot attribute every difference to the backstory, because model generation can vary even when the topic stays the same.

## 4. Choosing a framework

I would choose CrewAI for a project that turns a topic into an explanation through recognizable editorial roles. Researcher, writer, and editor are responsibilities that are easy to describe as agents, and each deliverable can be expressed as a task. For this assignment, changing one backstory produced a visible difference without rebuilding the workflow. The sequential crew also made it easy to follow intermediate work in the terminal.

LangGraph would be useful if I needed more explicit control over execution. The class starter represents the workflow through nodes, edges, and shared state. Its tool node runs requested tools and returns results to the agent. That structure would make it easier to design a specific branch or review loop, such as checking an answer and sending it back for revision.

For this simple writing pipeline, LangGraph would require more explicit workflow design than the CrewAI starter. CrewAI fits the role-based organization of the assignment, while LangGraph would be a stronger choice if custom execution paths became the main requirement. Neither framework alone guarantees factual accuracy or age-appropriate writing; those still require verification and suitable instructions.

## 5. Verification and AI use

Verification for this submission includes comparing the saved terminal records, final articles, and changed script. Both records contain the same topic, and the submitted script contains the new backstory. The toy-shop example appeared in the changed result, while unexplained technical terms remained. This supports partial success rather than complete compliance with the audience instructions.

This checks execution, relevance, and readability; it is not an independent fact-check. The kit has no web-search tools, so its research comes from the model's existing knowledge. Numerical claims and service recommendations in the articles have not been independently verified for this report. The `[source needed]` labels identify some uncertain claims, but an unmarked claim is not automatically reliable. I would check original sources before using either article for a real inventory decision.

I used ChatGPT to help interpret the assignment, select a topic, draft the revised backstory, understand the code, and draft this report from the saved files. The submitted code and actual run records provide the evidence for evaluating the AI-assisted work; AI assistance does not replace checking the output.
