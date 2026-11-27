---
title: "Event 3"
date: 2026-11-14
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# Summary Report: “Cloud Mastery”

### Event Objectives

- Understand the gap between an AI prototype and a production-grade enterprise AI system, and the role of the cloud in closing it.  
- Explore Amazon Bedrock AgentCore and the five services presented: Runtime, Identity, Gateway, Memory and Observability.
- See real-world use cases and a live demo of a multi-agent internal AI application built with Strands.  
- Learn common multi-agent and agentic patterns, and how RAG produces grounded answers.  

### Speakers

- **Khang Duong** – Developer, FCAJ member  
- **Quynh** – Developer, FCAJ member  
- **Bao Nguyen** – Developer, FCAJ member  
- **Lam Anh** – Developer, FCAJ member  
- **Khuong Huynh** – Developer, FCAJ member  

### Key Highlights

#### 1. The Gap: From Prototype to Production

- **The restaurant analogy**: A prototype is a home kitchen, enterprise AI is a restaurant chain, and the cloud is the central kitchen.  
- **AI in the enterprise**: AI is a feature inside the product (Data → AI Model → Feature → Users), for example recommendations, a support chatbot and fraud detection.  
- **Academic vs. enterprise AI**: Compared across four aspects: goal, data, users and operation.  
- **Four pillars of production AI**: Security, Reliability, Scalability and Cost.
- **Security in AI**: Data privacy, access control and AI-specific risks.  
- **Elastic scalability on AWS**: Lambda, Auto Scaling and Load Balancer.  
- **The role of the cloud**: Built-in security, scaling on demand, reliability across multiple Availability Zones, and pay-as-you-go cost.
- **AWS toolbox**: Lambda runs code on events with no servers to manage; Bedrock provides ready-made foundation models through an API; AgentCore operates AI agents (Runtime, Memory, Identity).  
- **Use AI wisely**: The "head chef" is an orchestrator running on a large model, while the "main course", "dessert" and "cashier" roles are handled by small, cheap models.  

#### 2. Real-World Use Cases and AgentCore Runtime

- **Use cases**: Real-life examples such as Cox Automotive and Mobileye.  
- **Where AgentCore fits**: The stack consists of the model, the agent application and Amazon Bedrock AgentCore.  
- **Building an agent with Strands**: Agents are written with the Strands framework.  
- **Running an agent with AgentCore Runtime**: The session covered developing an agent locally and then deploying it to AgentCore Runtime to manage its sessions.
- **Division of responsibility**: Developers own business rules, data and tools, and testing; the managed runtime provides compute, session isolation and scaling.  

#### 3. Identity, Gateway, Memory and Observability

- **Identity and Gateway**: AgentCore separates inbound from outbound access through a 5-step flow, and Gateway was covered as one of the remaining AgentCore services.  
- **Building context for an agent**: System prompt, documents, tools, instructions, memory and message history.  
- **Current challenges of agent memory**: Storage scalability, memory refresh, integration complexity, privacy and security concerns, retrieval accuracy problems and observability.  
- **AgentCore Memory**: Short-term memory (chat messages, session state) and long-term memory (semantic, user preferences, summarization, episodic and custom strategy).  
- **Memory flow**: The agent implementation sends events (messages) and reads memory (agent store, list events, retrieve memory records) from AgentCore Memory, and an automatic memory extraction module turns short-term events into long-term memory.  
- **Observability - "you cannot fix what you cannot see"**: Compliance, governance and traceability, lineage, cost visibility, troubleshooting and root cause analysis, understanding latency and operational performance, explainability and continuous evaluation.  

#### 4. Live Demo and Patterns

- **Live demo of an internal AI app**: User → FastAPI → Strands router, which routes each request to one of three agents: `qa_agent`, `ticket_agent` or `research_agent`.  
- **Common multi-agent patterns**: Routing/Supervisor, Sequential, Parallel and Evaluator-optimizer.  
- **Agentic pattern**: Reason → Act → Observe → Repeat or Finish.  
- **RAG - from documents to grounded answers**:  
    - *Indexing*: internal knowledge → embedding model → generate vectors → store in a vector store.  
    - *Querying*: user question → query embedding → retrieve the top-K relevant knowledge → add the retrieved context to the LLM prompt → generate a grounded answer with source references.  

### Key Takeaways

#### Design Mindset

- **AI is a feature of the product**: Production AI must be designed as part of a product with real users, not as a standalone model.  
- **Four pillars first**: Security, reliability, scalability and cost have to be planned from the start.  
- **Right-sized models**: Use a large model for orchestration and small, cheaper models for the rest of the work.  

#### Technical Architecture

- Deploy agents to a **managed runtime** so developers focus on business rules, data, tools and testing while the platform handles compute, session isolation and scaling.  
- Use **short-term and long-term memory** to keep context across messages and sessions.  
- Add **observability** (traceability, cost visibility, root cause analysis, continuous evaluation) before going to production.  
- Use **RAG** to ground answers in internal knowledge with source references.  

#### Production Strategy

- Pick the multi-agent pattern that matches the task: routing/supervisor, sequential, parallel or evaluator-optimizer.  
- Separate inbound and outbound access for agents to keep identity and permissions under control.  

### Applying to Work

- **Study AI security in depth**: Understand risks involving data privacy, access control and AI behavior before choosing additional security tools.
- **Practice the deployment flow**: Build a small agent with Strands and deploy it to AgentCore Runtime.
- **Use the four pillars as a checklist**: Review the security, reliability, scalability and cost of my own AWS architectures.  
- **Plan for observability early**: Add logging, tracing and cost visibility before deployment, not after.  

### Event Experience

Attending **Cloud Mastery** showed what it takes to move an AI application from prototype to production on AWS. Key experiences included:

#### Learning from the speakers
- Most speakers were developers and FCAJ members.  
- Followed three sessions: the gap between prototype and production, real-world use cases with AgentCore Runtime, and the remaining AgentCore services (Identity, Gateway, Memory and Observability).  
- The restaurant analogies (home kitchen, central kitchen, head chef) made the concepts easy to remember.  

#### Hands-on technical exposure
- Watched a live demo of an internal AI app that routes requests through FastAPI and a Strands router to three specialized agents.  
- Saw how multi-agent patterns and RAG fit into a working application.  

#### Exploring modern tools
- Learned the roles of Lambda, Bedrock and AgentCore, and how the Strands framework is used to build agents.  

#### Lessons learned
- I now have a broader overview of agentic AI.  
- I understand the steps involved in moving an AI agent from local development to a deployed application.
- A working model is not enough: production AI needs security, reliability, scalability and cost control from the start.  
- A managed runtime lets developers concentrate on business rules, data, tools and testing.  
- Memory supports continuity across interactions, while observability helps trace agent behavior and investigate issues.
- Choose model size according to the task and use smaller models when they meet the requirements at a lower cost.

#### Questions to explore further
- I need to study AI security further to understand the underlying risks before choosing tools or protective controls.

#### Some event photos
![Cloud Mastery Attendance Proof](/images/4-Event/cloud-mastery.jpg)

> Overall, Cloud Mastery gave me a broader view of agentic AI and a clear picture of how to take an AI from local development to a deployed application, while leaving me with security questions that I want to study further.
