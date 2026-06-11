# Week 11 Capstone Rubric: Cloud ETL Pipeline

**Assignment:** Build a Prefect-orchestrated ETL pipeline (`etl_pipeline.py`) that extracts weather data, classifies it with an LLM, and loads results to Azure Blob Storage.

**Scale:**
- **Does Not Meet Expectations** — requirement is missing or incorrect
- **Meets Expectations** — requirement fulfilled as described in the instructions
- **Exceeds Expectations** — requirement fulfilled with meaningful additions (better error handling, extra logging, thoughtful design choices, etc.)

**Deliverables:**
- `etl_pipeline.py`
- `outputs/pipeline_run.md` (4–6 sentence reflection)
- Video: terminal run + Prefect UI (all tasks Completed) + Azure Portal blob

---

## 1. Extract Task

| Criterion | Does Not Meet Expectations | Meets Expectations | Exceeds Expectations |
|---|---|---|---|
| `@task` decorator | Missing or incorrectly applied | `@task(retries=2)` present | Adds `retry_delay_seconds` or a descriptive task name |
| API call | Missing, wrong endpoint, or wrong parameters | Open-Meteo called for 7 days of hourly temperature + precipitation | Adds timeout, request headers, or validates response schema |
| Return value | Does not return raw JSON | Returns raw JSON response | |

---

## 2. Transform Task

| Criterion | Does Not Meet Expectations | Meets Expectations | Exceeds Expectations |
|---|---|---|---|
| `@task` decorator | Missing or incorrectly applied | `@task` present | Adds a descriptive task name |
| Data reshaping | Parallel lists not reshaped | Per-hour record dicts constructed correctly | |
| OpenAI classification | Missing or uses invalid label values | First 24 records classified as `good` / `marginal` / `bad` | Classifies more records or uses structured output parsing |
| Fallback handling | No fallback / crashes on unexpected response | Invalid responses default to `"unknown"` | Logs unexpected values before falling back |

---

## 3. Load Task

| Criterion | Does Not Meet Expectations | Meets Expectations | Exceeds Expectations |
|---|---|---|---|
| `@task` decorator | Missing or incorrectly applied | `@task` present | |
| Blob upload | Missing, wrong path, or wrong container | Enriched records uploaded to `final/<today>/weather_etl.json` | Uses `overwrite=True` safely or logs blob URL after upload |

---

## 4. Prefect Flow

| Criterion | Does Not Meet Expectations | Meets Expectations | Exceeds Expectations |
|---|---|---|---|
| `@flow` decorator | Missing or incorrectly applied | `@flow(log_prints=True)` present | Adds flow name or description |
| Task sequencing | Tasks not called or called out of order | Extract → Transform → Load in sequence | |
| Data handoff | Re-fetches data or uses globals | Output of each task passed as input to the next | |

---

## 5. Verification Artifacts

| Criterion | Does Not Meet Expectations | Meets Expectations | Exceeds Expectations |
|---|---|---|---|
| Prefect UI | Not shown or shows failed/crashed tasks | Video shows all three tasks in **Completed** state | Shows task duration or run logs in the UI |
| Azure Portal | Not shown or blob is at wrong path | Blob visible at `final/<today>/weather_etl.json` | |
| `pipeline_run.md` | Missing or empty | File present with 4–6 sentence reflection | |

---

## 6. Reflection

| Criterion | Does Not Meet Expectations | Meets Expectations | Exceeds Expectations |
|---|---|---|---|
| Length | Fewer than 4 sentences | 4–6 sentences as required | |
| Design reasoning | Vague or missing | Explains pipeline design choices or what they would change | Connects choices to real-world production concerns |
| LLM appropriateness | Not addressed | Reflects on whether LLM classification was the right tool for this task | Proposes an alternative approach with reasoning |
