---
title: "AI in Data Science: Capabilities and Limits"
description: "AI in Data Science: Capabilities and Limits"
pubDate: "Jul 08 2026"
---

Recent advances in AI have dramatically expanded machine capabilities, yet significant hype obscures their actual limitations. Understanding the core mechanics of AI clarifies its true impact on the data science profession.

Agentic AI systems take natural language inputs, process them via a core engine, typically a Large Language Model. They act as an orchestrator that infers intent, plans solutions, executes tasks via tools, receives feedback, and iterates until final synthesis.

While highly capable, LLMs face three distinct challenges:
1. Realtime data (e.g., what is today's weather?)
2. Private/proprietary data (e.g., proprietary company policy)
3. Complex workflows the model was not trained to execute (e.g., replacing a data scientist)

A common approach to resolve the first challenge is equipping the agent with APIs or web search. To address the second, agentic Retrieval-Augmented Generation (RAG) is often deployed to query private databases, providing the LLM with the necessary context.

The third challenge, however, is the primary hurdle. Agents are equipped with tools to execute tasks they cannot perform efficiently on their own, such as data summarization (via SQL) or visualization (via Python). Agents are typically trained to either write the necessary code or use tool descriptions to select the appropriate tool with simple arguments. But what happens when agents are given basic tools and tasked with building a complex workflow unlike anything they encountered during training? In most cases, they will fail.

We can effectively automate tasks where the output is easily evaluable (e.g., binary success/failure scores) using Reinforcement Learning from Verifiable Feedback (RLVF), which has proven successful in mathematics and coding.

However, tasks with ambiguous, subjective, or hard-to-verify outcomes cannot be easily automated. Current LLMs rely on brute-force compute and statistical correlations. As we exhaust internet data, future breakthroughs will require compact architectures that understand underlying mechanisms (e.g., Yann LeCun’s proposed "world models") rather than scaling compute indefinitely. Agentic AI bridges this gap by enabling LLMs to use tools, extending their context beyond parametric memory.

## Traditional Data Science
Will AI replace data scientists? No. While Agentic AI will automate repetitive execution, it will not eliminate the profession. Instead, increased productivity will shift the data scientist's focus toward high-value business optimization, strategic initiatives, and custom tool development.

The automation potential across the standard data science workflow breaks down as follows:
1. Data ingestion
	1. Problem: Ingesting new data requires reconciling discrepancies between metadata definitions and actual data reality, which traditionally demands manual, ad-hoc exploratory queries.
	2. Automation potential: By enforcing disciplined data engineering practices (clear schemas, documented assumptions), LLMs can execute diagnostic queries to validate documentation and automatically synthesize monitoring systems.
2. Data integration
	1. Problem: Consolidating scattered, multi-grain tabular data via SQL joins to create performant, analytical views.
	2. Automation: Given well-documented data layers and structured user prompts, LLMs can accurately generate initial integration prototypes. Eventually, fine-tuned models will automate the generation of requirements themselves.
3. Feature engineering
	1. Problem: Aggregating entity interactions over specific time horizons or structuring sequential streams for modeling.
	2. Automation: While agents can prototype features, unconstrained automation leads to redundant feature sets and massive management overhead. Features should remain centralized, version-controlled, and managed within a standard Feature Store.
4. Model development
	1. Problem: Iterative cycles of training, hyperparameter tuning, error analysis, and refinement.
	2. Automation: Full automation via unconstrained AutoML can cause an exponential explosion in cloud compute costs. Agents should serve as a productivity multiplier while data scientists monitor and constrain the loop.
5. Model deployment, observability and lifecycle managements
	1. Problem: Monitoring models for data drift or performance degradation, isolating root causes, and executing rollbacks/retraining.
	2. Automation: AI agents excel at rapidly prototyping standard error analyses and computing diagnostic metrics, drastically reducing human turnaround time during incidents. 

The following subsections expand on each of these points.

### Data Ingestion
Data ingestion ranges from straightforward to highly complex. Simple ingestion pipelines, such as appending data to an existing table with automated QA checks, can be easily automated. However, data is frequently stacked without a clear end-use case in mind. When integrating a new data source, one must understand both the new data asset and how it fits into the broader enterprise data ecosystem. Traditionally, data scientists have spent significant time manually exploring existing tables and running ad hoc queries to identify discrepancies between what the metadata claims to represent versus what it actually represents.

The first step toward automating data ingestion is implementing principled data engineering practices. This requires explicitly defining the purpose of the data, providing detailed field descriptions, documenting architectural and data integrity assumptions, and listing known edge cases or anomalies. Once established, existing data sources must be continuously monitored to ensure these assumptions remain valid over time.

Can this process be automated? Likely, yes. We can provide an LLM with the current metadata and task it with executing diagnostic queries to verify and enrich the documentation. The agent can then systematically reason about potential failure modes, iterating to expand the documentation. Once the documentation reaches a complete state, the LLM can synthesize the corresponding monitoring system. Over the long term, these capabilities will likely become native features of LLMs, cloud platforms, or database providers. In the interim, businesses should focus on building targeted, stopgap agentic workflows to fill this gap.

### Data Integration
After data is ingested, the necessary information is often scattered across different assets and must be combined. Traditionally, tabular data is preferred because it is highly accessible and SQL talent is abundant. Raw tables are typically joined to consolidate information into a manageable number of tables while maintaining the required granularity, making the data easier to consume and more performant for analysts.

Like data ingestion, this consolidation process can be automated. While organizations differ, once users adapt to formatting prompts that lay out specific requirements, and provided that the underlying data is well-documented, an LLM can generate initial prototypes that require minimal modification. Furthermore, once a sufficient volume of these structured requirements is collected, an LLM can be fine-tuned to automate the generation of the requirements themselves.

### Feature Engineering
Feature engineering is inherently tied to model building because its output directly serves as the model input. Many models expect each row to represent a specific target entity (e.g., advisors, funds, investors). The interactions an entity makes with others are then aggregated to create predictive features (e.g., number of transactions in the past month, current portfolio composition). In some cases, aggregations are computed relative to specific timestamps, meaning each row represents a unique tuple of an entity and a point in time. Alternatively, when sequence models are used, the raw interactions of an entity are fed directly into the model as a sequential stream without aggregation. Even then, environmental representations (e.g., inflation and interest rates at the time of the transaction) must be joined to each entity object within the sequence.

While this step can be automated via agents, it is often better not to fully automate it. AI can certainly be leveraged to prototype initial features, but generating distinct feature sets independently for every model introduces redundancy and management overhead. Within any given organization, there are typically only a limited number of entity grains and broad modeling strategies. Many features can and should be shared across different models. Rather than letting agents generate them ad hoc, each feature should be systematically documented and managed within a centralized feature store to track version changes and maintain the exact replication logic.

### Model Development
Model development is an iterative process. Once the modeling target and strategies are selected, data scientists fit candidate models and perform hyperparameter tuning, a phase historically lampooned as "graduate student descent," though modern neural architecture search (NAS) techniques have mitigated the problem. When no candidate model performs satisfactorily, error analysis is conducted to diagnose failure modes. Models are then refined by detecting code bugs, engineering new features, or collecting more targeted data based on these diagnostics.

While many parts of this loop can be automated, a major caveat is that unconstrained autoML can yield an exponential explosion of experiments, significantly inflating cloud and development costs. Rather than fully automating the pipeline, a data scientist should monitor the loop. Even with this human-in-the-loop constraint, agents will provide a significant productivity boost.

### Model Deployment, Observability and Lifecycle Management
Once a model is developed, an appropriate deployment strategy must be selected. Then, data and model performance must be continuously monitored for drift (shifting distributions over time). When direct model performance is difficult to compute, such as when data labeling is costly, proxy metrics correlated with the target variable can be monitored instead.

Traditionally, when performance degradation or significant data drift is detected, data scientists manually query the data to isolate the root cause. If a model requires rollback or retraining, data scientists should be able to execute these actions seamlessly via preexisting pipelines.

AI agents can assist by prototyping standard error analyses and rapidly computing diagnostic metrics, thereby reducing turnaround time.

## New Approaches
### Self-Supervised, Transfer and Reinforcement Learning
While Large Language Models (LLMs) dominate the headlines, the underlying methodologies driving their success are often overlooked.

An LLM is first trained on vast amounts of internet text to predict the next word in a sentence. Because the text itself provides the answer, this process requires no manual labeling, allowing the model to develop a broad understanding of language. However, predicting the next word does not guarantee the model will be helpful. To ensure it accurately follows human requests, it undergoes instruction tuning using curated query-and-answer datasets. Finally, the model is refined through reinforcement learning, a trial-and-error process where it generates answers, receives feedback from human or AI judges, and self-corrects based on those evaluations.

This same architecture can be applied to financial applications, such as fund recommendations. Because individual investors purchase funds infrequently, traditional data is sparse. We can solve this by treating an investor's history as a sequence of interactions, where each action (a click, a call, a view) acts like a "word." By training a model to predict the next interaction, it gains a foundational understanding of investor behavior without needing specialized labels.

From there, we can introduce specific interventions, like pop-up recommendations, chatbot invites, or advisor sessions, and define a clear reward, such as a fund purchase within 30 days. Using reinforcement learning, the model learns to favor actions that yield high rewards and phase out those that do not.

Since this approach relies on historical correlations, real-world testing is essential. Deploying the model to a small subset of live users provides direct feedback, allowing it to continuously adapt and maximize performance over time. Ultimately, this creates a single, highly adaptable base model that captures how investor behavior evolves, which can then be efficiently customized for various business goals.

### Representation Learning
Recent breakthroughs allow us to translate almost any medium (text, images, or audio) into vectors. This makes it easy to inject rich, diverse information directly into predictive models.

For example, a fund's net flow often shifts after a corporate earnings call. By converting the text transcript of that call into a vector, a predictive model can instantly digest its context and improve its forecasts.

Furthermore, advanced models can map different types of media into a single, shared space where text, audio, and images can be compared directly. This enables the creation of multimodal systems that analyze major news, earnings transcripts, and macroeconomic shocks together to predict fund performance.

### Graph Neural Network
Another powerful development is the Graph Neural Network (GNN), which naturally maps complex networks of multi-type interactions (like calls and purchases) between different entities (such as advisors, funds, and investors). GNNs serve as a unifying framework that can model all of these moving parts simultaneously.

While a single, unified model might seem less precise than a collection of specialized ones, AI history suggests otherwise. In language processing, training a single model on multiple languages actually improves its performance in each individual language. When different financial domains share underlying behavioral patterns, training them together makes the entire system smarter.

### Causal Inference and Decision Science
Traditional experiments and hypothesis testing are inherently asymmetrical and rigid. You must wait until the end of a trial to draw conclusions, and even then, you only find a single, flat average effect. You are simply rejecting or accepting a baseline assumption, rather than directly comparing competing options.

Developments in causal inference and decision science change this entirely. Modern techniques like anytime-valid inference and e-values eliminate the need to wait for a fixed sample size, allowing for flexible, real-time data analysis. Simultaneously, approaches like multi-armed bandits with Thompson sampling enable adaptive experiments where multiple variations run concurrently and inferior strategies automatically phase out. When the combination of experimental factors becomes overwhelmingly complex, active learning seamlessly steps in to determine exactly which data points the system should target next to optimize the learning rate.

Causal machine learning moves us from blanket averages to personalized experiences by uncovering heterogeneous treatment effects, the reality that different people react differently to the same stimulus. Instead of running separate, muddy trials, a single modern experiment can isolate which specific customer segment responds best to an intervention, such as a tailored landing page or an advisor call, ensuring high-impact personalization without losing causal clarity.