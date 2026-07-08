# Chapter 2 - Lab 4: Advanced Prompting Techniques (ToT and ReAct) A/B Comparison

In this study, we examine how artificial intelligence can reason more deeply by using different techniques in complex problems that do not have a single correct answer.

## Comparison Scenario: Scalable Data Architecture Design

**Task:** "Our company needs to process 100 million rows of streaming data per day and deliver it to the analytics team with sub-second latency. Low cost is a must. Create the architecture design."

### Test A: General Step-by-Step Prompt (Standard CoT)
In our first attempt, we gave the AI a straightforward step-by-step instruction: "first understand the problem, then select the components, and finally create the architecture". 
The model immediately suggested using Kafka and Snowflake. The architecture made sense, but it completely overlooked the fact that costs could be extremely high and didn't try a different cloud architecture. It acted as if there was "only one correct answer".

### Test B: Selection with Tree of Thoughts (ToT)
In this scenario, since we know there is no single right answer to the problem, we used the **Tree of Thoughts (ToT)** approach:

**Prompt Structure:**
> "Step 1: Produce 3 completely different data architecture scenarios (one AWS native, one Open-Source based) that will meet these requirements (Branching).
> Step 2: Critically evaluate each architecture across the axes of "Performance", "Cost", and "Maintenance Difficulty" (Evaluation).
> Step 3: Select the 2 most suitable architectures and present a hybrid optimal system recommendation (Convergence)."

**Result and Analysis:**
Contrary to the superficial answer (A) in the first attempt, the model;
1. Offered AWS Kinesis + Redshift as an option,
2. Offered Apache Kafka + ClickHouse as an option,
3. Offered Pub/Sub + Athena as an option.
Then, it eliminated those of its own solutions that did not fit the "Cost" constraint and synthesized the most optimal structure (Kafka + ClickHouse) for us. 

**The biggest difference:** With the ToT approach, the AI acted like a jury by establishing multiple hypotheses and cross-evaluating them (criticizing its own ideas) instead of jumping to the "first solution it found".

## A/B Usage Criteria (My Application Rules)

By synthesizing these findings, I have determined my own "Golden Rule" to use in my future projects as follows:

> "If the task is a simple text translation, code explanation, or writing a routine report, I will use **Prompt A (Standard / Zero-Shot)**. 
> However, if the task is a problem that does not have a single correct answer and requires multi-dimensional decision-making, such as architectural design, campaign strategy, or debugging, I will definitely use **Prompt B (ToT or ReAct)** to eliminate the model's blind spots. Thus, I ensure that the model cross-examines itself to guarantee that AI results are always more deeply analyzed, reliable, and consistent."
