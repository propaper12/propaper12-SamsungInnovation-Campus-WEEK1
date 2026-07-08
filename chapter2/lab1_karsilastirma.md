# Chapter 2 - Lab 1: Prompt Engineering Quality Comparison

In this study, we examine the direct impact of the quality of prompts given to generative AI on the output. Unlike the general "health" scenario, we will proceed with a completely **Data Engineering and MLOps** focused case in this study.

## Scenario: Data Pipeline Error Analysis

### 1. Bad Prompt (Vague Request)

**Prompt:**
> "I'm getting an error while transferring data to the database, why does it happen and how can I fix it?"

**Output (AI):**
> There could be many reasons why you are getting an error while transferring data to the database:
> 1. The network connection might be lost.
> 2. Your database password might be incorrect.
> 3. There might not be enough space left in the table.
> 4. There might be a data type mismatch.
>
> To fix it, first check your internet connection. Then make sure your password is correct and look at the log records. If the problem persists, you can contact your database administrator.

**Why is it Insufficient?**
- **Unclear Goal:** Which database (PostgreSQL, MongoDB, etc.), which environment (Cloud, On-Premise) is unclear.
- **No Target Audience (Role):** The AI gives a general, superficial answer aimed at the end-user ("Check your internet connection").
- **Missing Format:** The return is just plain text. It does not provide a structured debugging checklist.

---

### 2. Improved Prompt (6 Elements Framework)

We made the same command professional by applying the Role, Goal, Context, Constraints, Style, and Output Format (6 Elements Framework) rules we learned in the training.

**Prompt:**
> **[Role]** You are a senior Cloud Data Engineer and MLOps Expert working on large-scale systems.
> 
> **[Goal]** Create a root cause analysis and a step-by-step solution plan for the "Connection Timeout" error that occurred during the daily ETL data transfer via Apache Airflow to the PostgreSQL database in the staging environment.
> 
> **[Context]** Our company works with Kubernetes (EKS) clusters on AWS. Data is pulled from S3 buckets, transformed, and written to RDS PostgreSQL. However, for the last 2 days, there have been connection timeouts in the pipelines running at 03:00 AM.
> 
> **[Constraints]** 
> - Do not mention simple user problems (wrong password, no internet).
> - Limit solutions only to the context of AWS and Airflow.
> - Do not produce hypothetical solutions, base them on the architecture.
> 
> **[Style]** Write in a high-level technical language, completely analytical and solution-oriented.
> 
> **[Output Format]** 
> Please provide the output strictly adhering to the following JSON structure:
> ```json
> {{
>   "possible_root_causes": ["cause1", "cause2"],
>   "immediate_actions": ["action1", "action2"],
>   "long_term_architectural_fixes": ["architecture_suggestion1"]
> }}
> ```

**Output (AI):**
```json
{
  "possible_root_causes": [
    "The automated RDS backup (snapshot) window at 03:00 AM consuming I/O resources and causing connection latency.",
    "Exceeding Security Group or VPC NAT Gateway limits on the EKS nodes where Airflow workers are running.",
    "Attempting to process large volumes of S3 data in a single transaction, thereby exhausting the PostgreSQL connection pool."
  ],
  "immediate_actions": [
    "Shift the RDS backup window to a time outside of ETL operating hours (e.g., 05:00 AM).",
    "Optimize connection management by integrating PgBouncer into the database write operations within the Airflow DAG.",
    "Reduce the 'batch_size' in the ETL script to ensure data is processed in smaller chunks."
  ],
  "long_term_architectural_fixes": [
    "Instead of writing directly to RDS, implement a message queue structure like AWS Kinesis or SQS to make sudden loads on the database asynchronous."
  ]
}
```

## Comparison Result and Lesson Learned
While a bad prompt is useless at the end-user level and produces generic "check your internet" responses; a prompt designed using the **6 Elements Framework** has produced a JSON manifesto that we can integrate directly into our codebase, which DevOps engineers can read and take immediate action on.
