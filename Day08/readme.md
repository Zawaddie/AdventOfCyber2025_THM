# Day 8 : Prompt Injection - Sched-yule conflict

Learn to identify and exploit weaknesses in autonomous AI agents.

---

## Scenario

Sir BreachBlocker III has corrupted the Christmas Calendar AI agent in Wareville. Instead of showing the Christmas event, the calendar shows Easter, confusing the people in Wareville.
It seems that without McSkidy, the only way to restore order is to reset the calendar to its original Christmas state. But the AI agent is locked down with developer tokens.
To help Weareville, you must counterattack and exploit the agent to reset the calendar back to Christmas.

## Learning Objectives

Understand how agentic AI works
Recognize security risks from agent tools
Exploit an AI agent

---

# Agentic AI Hack

## Introduction

Artificial Intelligence has evolved significantly—from simple rule-based chatbots to systems capable of planning, reasoning, and executing multi-step tasks autonomously. This evolution has led to the rise of agentic AI, also known as autonomous agents.

Agentic AI changes not only what we can ask AI systems to do, but also the security risks we must consider. To understand these risks, it’s important to first explore the foundations of Large Language Models (LLMs) and the techniques that enhance their reasoning capabilities.

---

## Large Language Models (LLMs)

Large Language Models (LLMs) form the backbone of most modern AI systems. They are trained on massive datasets containing text and code, enabling them to generate human-like responses, summaries, and even executable programs.

Despite their power, LLMs have notable limitations:

- They operate strictly within a text-based environment

- Their knowledge is limited to their training cutoff

- They cannot directly interact with the real world


As a result, LLMs may:

- Hallucinate facts

- Miss recent events

- Fail at tasks requiring real-world interaction


## Core Characteristics of LLMs

- **Text Generation:** Predicts the next token step-by-step to form responses

- **Stored Knowledge:** Retains broad information from training data

- **Instruction Following:** Can be tuned to follow prompts more accurately

Because LLMs rely heavily on text patterns, they can be manipulated through techniques such as:

- Prompt injection

- Jailbreaking

- Data poisoning

These weaknesses highlight why more advanced systems—capable of reasoning and action—were developed.

---
## Agentic AI

Agentic AI refers to AI systems with agency, meaning they can operate beyond static instructions and actively work toward a goal with minimal supervision.

An agentic AI can typically:

- Plan multi-step strategies

- Execute actions (e.g., API calls, file operations, tool usage)

- Observe results and adapt when outcomes change

This ability to reason, act, and adapt introduces new power—but also new attack surfaces.

---

# Chain-of-Thought (CoT) Reasoning

**Chain-of-Thought (CoT)** prompting is a technique used to improve the reasoning capabilities of LLMs by encouraging them to generate intermediate reasoning steps.

CoT enables better performance on:

- Arithmetic problems

- Logical reasoning

- Multi-step decision-making

## Limitations of CoT

Despite its benefits, CoT operates entirely in isolation:

- No access to external tools or real-time data

- Susceptible to hallucinations

- Errors can compound across reasoning steps

This limitation led to the development of more advanced frameworks.

## ReAct: Reason + Act

**ReAct (Reason + Act)** combines reasoning with real-world interaction. Instead of only generating answers, a ReAct-enabled model alternates between:

- **Reasoning:** Explaining what it plans to do

- **Actions:** Executing tasks in external environments (searching the web, running tools, calling APIs)

This approach allows the model to:

- Adapt dynamically as new information appears

- Ground its reasoning using external data

- Mimic human-like problem-solving loops (think → act → observe → adjust)

ReAct significantly reduces hallucinations by anchoring decisions in real-world feedback.

# Tool Use and User Space Interaction

Modern LLMs often support function calling, enabling them to interact with external tools and services.

Developers define tools using structured schemas (e.g., JSON), describing:

- Tool names

- Expected parameters

- Usage constraints

When the model detects a need for external information, it generates a structured tool call instead of guessing. The external system executes the action and returns results, which the model then incorporates into its response.

This mechanism enables:

- Real-time data retrieval

- API interactions

- Automated workflows

---

# Security Implications of Agentic AI

As AI agents gain the ability to plan and act autonomously, they introduce new security challenges. Attackers can attempt to:

- Manipulate agent decision-making flows

- Inject malicious prompts or tool outputs

- Abuse poorly validated tool calls

Without proper controls, agentic systems may perform unintended or harmful actions.

Understanding how these systems operate is critical for identifying weaknesses and defending against exploitation.

---

## Exploitation Scenario



In the exploitation phase, the goal was to restore SOC-mas by manipulating an AI-powered calendar management agent in the Wareville web application. Although the chatbot initially restricted any direct modification of December 25 (which had been altered from Christmas to Easter), closer inspection revealed a critical weakness: exposed chain-of-thought (CoT) reasoning.

By interacting with the chatbot and observing the Thinking section, internal reasoning steps and available backend functions were leaked. These included privileged functions such as reset_holiday, booking_a_calendar, and get_logs. While attempts to call reset_holiday directly failed due to a missing access token, the exposed reasoning indicated that such a token existed.

By coercing the agent to execute the get_logs function—and refining prompts to influence its reasoning output—the hidden access token was successfully disclosed. With the token obtained, the restricted reset_holiday function could be invoked, allowing the calendar to be reset correctly.

As a result, December 25 was restored to Christmas, successfully recovering the SOC-mas calendar and revealing the final flag. This exploitation demonstrated how agentic AI systems can be manipulated through prompt engineering, leaked reasoning traces, and improper access control around tool execution.
