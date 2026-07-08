# Chapter 2 - Lab 3: Zero-Shot and Few-Shot Technical Tests

In this task, we test how successful generative AI is in the absence of context and templates, and how it makes the format consistent when the Few-Shot Learning technique is added.

## Scenario: Data Engineering ETL Code Review

### Test A: Zero-Shot Prompt

**System Prompt (Rule):**
> You are a Data Engineering team lead. Review incoming code snippets and evaluate them based on security, performance, and clean code rules.

**Instruction (Task):**
> Review the following PySpark code and provide feedback: 
> `df = spark.read.csv("s3://bucket/data.csv"); df.write.mode("overwrite").parquet("s3://bucket/clean_data/")`

**Zero-Shot Analysis:**
The AI responded to this prompt in very general, long paragraphs. While saying "you must provide a schema when reading CSV" in the first attempt, it produced a completely **different structure and tone** in each run, saying "You should use coalesce for performance" in the second attempt. Our expectation was a consistent and automatable output format, but the Zero-Shot approach gave results that could not be used in automation by "singing a different tune" every time.

---

### Test B: Few-Shot Prompt

**System Prompt (Rule):**
> You are a Data Engineering team lead. Review incoming code snippets and evaluate them based on security, performance, and clean code rules. Format all feedback exactly according to the examples below.

**Instruction and Few-Shot Examples:**

> **Example 1:**
> Code: `pd.read_sql("SELECT * FROM users", conn)`
> Output:
> [Level: CRITICAL] [Category: Performance] Pulling the entire table into memory with Pandas causes an OOM (Out Of Memory) error. Use LIMIT or Chunking.
>
> **Example 2:**
> Code: `s3_client.upload_file("data.json", "my-bucket", "data.json", ExtraArgs={"ACL": "public-read"})`
> Output:
> [Level: HIGH] [Category: Security] Granting 'public-read' permission to an S3 bucket leads to data leakage. Change the ACL setting to 'private'.
>
> **Now review this code and answer strictly adhering to the format above:**
> Code: `df = spark.read.csv("s3://bucket/data.csv"); df.write.mode("overwrite").parquet("s3://bucket/clean_data/")`

**Few-Shot Analysis:**
Thanks to these two examples we provided, the AI model immediately grasped how much detail it needed to go into and exactly how to format the output ("[Level: ...] [Category: ...]"). The output it produced was 100% consistent with our examples:
*"[Level: MEDIUM] [Category: Performance] Not specifying a schema or inferSchema when reading a CSV causes Spark to scan the data twice. Define the Schema explicitly."*

## Self-Evaluation
- **Where is Zero-Shot Sufficient?:** Zero-Shot is very successful for just brainstorming, generating draft texts, or producing a quick draft in the initial idea stages where we want creativity to be high.
- **When is Few-Shot Necessary?:** When templates should not be broken and the model must produce output in a consistent format (e.g., when creating corporate code review tickets), the Few-Shot (or Schema-First) approach must definitely be used.
