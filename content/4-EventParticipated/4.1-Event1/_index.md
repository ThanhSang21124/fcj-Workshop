---

title: "Event 2"
date: 2026-05-24
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
----------------------

# Reflection Report: AWS Vietnam Community Day 2026

### Event Objectives

* Learn about the latest trends in AI, GenAI, Agentic AI, and Cloud Computing.
* Gain insights into real-world AI implementations in enterprises.
* Explore Multi-Agent architectures and AWS solutions for AI workloads.
* Understand security, guardrails, and compliance in modern AI systems.
* Connect with developers, architects, and students who are passionate about technology.

### Speakers

* **Pham Ng Hai Anh** – AWS Community Builder
* **Nguyen Tuan Thinh** – DevOps Engineer
* **Tinh Truong** – Platform Engineer, GoTymeX
* **Vy Lam** – Senior Business Systems Analyst, VPBank
* **Duc Dao** – Solution Architect, Cloud Kinetics
* **UTMorpho Team** – Hackathon Project Team

### Highlights

#### 1. Amazon Q Business – Agentic AI Assistant for Enterprises

**Speaker:** Pham Ng Hai Anh – AWS Community Builder

This session introduced Amazon Q, a unified Agentic AI platform designed to help business users work more efficiently. Instead of manually gathering information from multiple sources, Amazon Q can:

* Connect to more than 40 data connectors, files, and databases.
* Utilize Amazon Bedrock models, web search, and thousands of actions.
* Automatically generate meeting summaries, send emails, schedule meetings, analyze data, and create dashboards.

#### 2. Amazon CloudFront – Foundation from Edge to Origin

**Speaker:** Nguyen Tuan Thinh – DevOps Engineer

The presentation explored how Amazon CloudFront serves as a foundation for performance, security, and cost optimization.

* Reduce costs related to data transfer, load balancing, and EC2 workloads.
* Improve security through Origin Cloaking, VPC Private Origin, OAC, Mutual TLS, Signed URLs, and Geo-restriction.
* Enhance performance with HTTP/3 (QUIC), multi-layer caching, Origin Shield, and HTTP compression.

#### 3. Context Is Everything – Making AI Truly Effective

**Speaker:** Tinh Truong – Platform Engineer, GoTymeX

This was one of the most insightful sessions about using AI effectively.

Key takeaways included:

* Context quality is more important than context quantity.
* Common mistakes when prompting AI.
* A practical framework consisting of:

  * Goal
  * Relevant Information
  * Constraints
  * Success Criteria
* The future evolution of AI from Prompt → Context → Memory (Second AI Brain).

#### 4. Enterprise-Grade Multi-Agent System

This presentation introduced a Multi-Agent AI System designed for Startup Credit Scoring in the financial sector.

##### Challenges in Traditional Systems

* Startups often lack:

  * Credit history
  * Long-term financial statements
  * Clear collateral
* Data is often unstructured, multidimensional, and rapidly changing.

##### Multi-Agent Architecture

The system consists of specialized agents:

* Financial Analyst
* Market Analyst
* Team Evaluator
* Risk Assessor
* Compliance Agent

##### Benefits of Multi-Agent Systems

* Domain specialization
* Parallel processing
* Better auditability and traceability
* Higher fault tolerance than single-agent systems
* Easier scalability

#### 5. Enterprise AI & Security + LLM Non-Determinism

**Speaker:** Duc Dao and other presenters

The session emphasized that enterprise AI systems must be:

* Secure
* Reliable
* Scalable
* Compliant

It also explained why Large Language Models can remain non-deterministic even when `temperature = 0`.

##### Main Causes

* Floating-point arithmetic on GPUs
* Parallel execution order
* Inference batching by providers

##### Mitigation Strategies

* Structured outputs
* Majority voting
* Ensemble approaches
* Thorough testing

#### 6. Hackathon Project – UTMorpho

This session showcased an AI UI Generator project developed during the LotusHacks Hackathon.

##### Main Idea

The AI UI Generator allows users to:

* Generate UI from prompts
* Edit interfaces directly
* Avoid repeated prompting
* Maintain consistency across modifications

##### Challenges Faced

* Token limitations
* Burnout during the Hackathon
* AI overgeneration
* Time pressure

##### Lessons Learned

* Team chemistry is extremely important.
* Real frustrations often inspire real solutions.
* AI should be treated as a teammate rather than just a tool.

### What I Learned

#### Architecture Mindset

* Multi-agent systems are suitable for complex enterprise applications.
* Context Engineering is a critical skill in the AI era.
* Security and compliance should be considered from the beginning.
* CloudFront provides a strong foundation for performance and cost optimization.

#### AI Knowledge

I gained a deeper understanding of:

* LLM inference
* Non-determinism
* Guardrails
* Structured outputs

#### Cloud & AWS Knowledge

* Amazon Q
* Amazon Bedrock Guardrails
* CloudFront
* Origin Shield
* OAC
* HTTP/3
* Edge Computing

#### Practical Skills

* Building AI production systems
* Designing scalable architectures
* Applying a business-first and context-driven mindset when working with AI

### Applications to Study and Work

* Apply the Context Framework when using AI for learning and software development.
* Use Amazon CloudFront to improve application performance and reduce costs.
* Experiment with building a mini Multi-Agent System or a Personal Second AI Brain.
* Integrate Bedrock Guardrails and structured outputs into AI applications.
* Apply microservices, event-driven, and domain-driven architectural patterns to personal projects.

### Event Experience

Attending **AWS Vietnam Community Day 2026** was an inspiring and valuable experience. The event not only introduced the latest developments in AI and Cloud Computing but also provided practical insights into how organizations deploy AI solutions in production environments.

#### Learning from Industry Experts

The speakers shared valuable real-world experiences related to:

* Multi-Agent AI Systems
* Agentic AI
* CloudFront Architecture
* Context Engineering
* Enterprise Architecture
* AI Security and Compliance

#### Expanding My AI Knowledge

I was particularly impressed by the following topics:

* LLM Non-Determinism
* Context Is Everything
* GenAI Guardrails
* Agentic capabilities of Amazon Q

#### Technology Community Experience

The event brought together developers, architects, and students who share a passion for technology, creating a highly engaging learning environment.

Through networking and technical discussions, I learned:

* Practical system design thinking
* Challenges of deploying AI in enterprise environments
* Experiences from Hackathons and rapid MVP development

#### Lessons Learned

* Enterprise AI requires security to be considered from the start.
* High-quality context is the key to unlocking AI's full potential.
* Multi-agent systems are effective for solving complex problems.
* Teamwork and communication are essential for successful projects.

> Overall, AWS Vietnam Community Day 2026 expanded my knowledge of AI and Cloud Computing while motivating me to continue exploring and developing innovative technology projects in the future.
