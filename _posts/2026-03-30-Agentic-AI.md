---
title: A Glance at Agentic AI
tags: [Glance, Tech Blog, Article]
style: fill
color: success
description: My first exposure to agentic workflows
---

In this post, I share some key learnings and personal "hacks" which I found helpful while building my first agentic workflow.

#### The key steps in any agentic workflow
1. Planning
2. Tool-calling
3. Reflection
4. Evaluation

#### What calls for a standalone-agent?
Initially, I thought it best to provide all the tools to a single LLM, and ask it to execute the entire workflow, i.e. call tools accordingly, as this would reduce the number of agents and thus the overhead of coordinating communication between them.

But doing so reduces the "expertise" of an agent, by which I mean:
1. Context-window overload
2. Different LLMs have different capabilities, so this would not harness that strength.

#### The importance of writing good prompts
The wording and structure of a prompt really determines how good a response the LLM gives.
Through trial-and-error, I follow these mental guidelines while instructing an LLM:
1. Persona<br>
    Give the LLM a persona to tell it what it excels in to help it interpret inputs and give outputs according to said expertise.

2. Input and output format <br>
    Clearly mention key components of the input and descriptions for them - especially for the output - so that the LLM gives outputs that can be easily parsed by the program later.
    
    I found that JSON objects are very powerful output formats as it helps me extract different components of my response easily.

3. Constraints<br>
    List any constraints/rules that the LLM must adhere to (What websites or tools it shouldn't use) when responding to a prompt - this helps it give "safer" answers that suit your needs. Additionally, using a tool like the BeeAI Framework can help enforce these rules programatically, rather than relying on the LLM itself to stick to the constraints.

4. Examples<br>
    This gives the LLM the idea of what you're looking for. 
    Few-shot prompting always betters the output - so I give clear input, reasoning, and output examples to help it respond better.

5. Final instructions<br>
    This is where I emphasise important instructions like "Give output only in JSON format, without extra commentary" - and I write this in all-caps for special effect ;)

#### Monitoring token usage
While the above steps are great in extracting good responses, it can take up a lot of tokens. Here are some tiny ways I try to better use tokens:
1. Try to condense (slightly long) groups of words into a single term

2. After an experimentation phase, if I know my LLM is meant for a specialized usecase that requries some expertise - I would apply parameter-efficient fine-tuning (PEFT) methods to avoid giving large system prompts that describe how it should behave. (I am currently experimenting wit this.)

3. While I personally haven't tried, I have read about soft-prompting reducing both token-usage and enabling the usage of LLMs with different expertises in a plug-and-play architecture.
In simple words as I understand it, soft-prompting is the process of representing a text-prompt in terms of trainable parameters that can be "connected" and "disconnected" from an LLM.

#### Evaluation - A crucial step
Finding ways to evaluate your agent's response is key as it can give you insight into where you can make it better - from improving the tools it uses, to the restructuring the prompt.
