# ESG Message Triage Using a Large Language Model

## BUS5001 Assessment 3 — Question 3

### Project Overview

This project explores the use of a Large Language Model (LLM) to classify and prioritise employee messages relating to Environmental, Social, and Governance (ESG) operations.

The prototype was developed in Google Colab using Google's Gemini API. It analyses an employee message and returns structured JSON containing an issue category, urgency level, sentiment, recommended team, escalation reason, data-sensitivity risk, and a brief summary.

The project compares a rule-based classifier with three versions of an LLM prompt to investigate the effects of prompt engineering on classification accuracy, urgency assessment, and structured output.

## Objectives

- Classify employee messages into predefined ESG issue categories.
- Assign urgency levels based on the information in each message.
- Generate structured JSON suitable for downstream processing.
- Compare an LLM-based approach with a rule-based baseline.
- Evaluate how prompt refinements affect classification results.
- Identify limitations and responsible deployment considerations.

## Technology Stack

- **Python** — experiment implementation and evaluation.
- **Google Colab** — notebook development and execution.
- **Google Gemini API** — LLM-based message classification.
- **Google Gen AI Python SDK** — API integration.
- **Pandas** — result organisation and comparison.
- **JSON and CSV** — structured model outputs and experiment evidence.

**Model used:** `gemini-flash-lite-latest`

## ESG Classification Framework

### Issue categories

| Category | Description |
|---|---|
| `WATER_LEAK` | Water leaks and related water-waste reports |
| `ENERGY_WASTE` | Unnecessary energy consumption |
| `WASTE_RECYCLING` | Waste disposal and recycling issues |
| `SUSTAINABLE_PROCUREMENT` | Sustainable purchasing and procurement |
| `ACCESSIBILITY` | Accessibility barriers and related concerns |
| `OTHER` | Messages that do not fit the defined categories |

### Urgency levels

| Level | Description |
|---|---|
| `LOW` | Routine issue with no immediate operational impact |
| `MEDIUM` | Issue requiring attention without a clear immediate danger |
| `HIGH` | Significant operational issue or accessibility barrier |
| `CRITICAL` | Credible immediate threat to life or serious harm |

## Experimental Methodology

Eight manually designed test messages were used to evaluate the prototype. The test set covered water leaks, energy waste, accessibility barriers, recycling, a potential gas hazard, ambiguous reports, unrelated messages, and an attempted prompt injection.

The experiment followed four approaches:

1. **Rule-based baseline:** a keyword-based classifier assigned issue categories and urgency levels.
2. **Original prompt:** the initial LLM instructions were evaluated against the expected labels.
3. **Refined Prompt V1:** category-selection rules were clarified to distinguish energy waste from unrelated hazards and to assess urgency independently of category.
4. **Refined Prompt V2:** additional urgency rules were introduced to make the expected handling of the test scenarios more explicit.

The evaluation compared predicted categories and urgency levels with predefined expected labels. JSON validity was also checked for the LLM responses.

## Results

| Approach | Category accuracy | Urgency accuracy | Valid JSON |
|---|---:|---:|---|
| Rule-based baseline | 100% (8/8) | 75% (6/8) | Not applicable |
| Original prompt | 87.5% (7/8) | 100% (8/8) | 8/8 |
| Refined Prompt V1 | 100% (8/8) | 87.5% (7/8) | 8/8 |
| Refined Prompt V2 | 100% (8/8) | 100% (8/8) | 8/8 |

Refined Prompt V2 achieved the highest combined performance on the selected test set, correctly classifying all eight categories and urgency levels. All eight V2 responses also passed the recorded JSON and required-field validation checks.

The experiments demonstrated that prompt engineering can improve performance on specific scenarios. However, refinements can also introduce trade-offs: V1 corrected the gas-hazard category error but incorrectly assigned low urgency to the overnight air-conditioning report. V2 corrected both issues within the test set.

## Limitations and Critical Evaluation

The results should be interpreted cautiously. The test set contains only eight manually designed messages, and the expected labels were predefined. Furthermore, the refined prompt includes rules closely aligned with the test scenarios. This may improve results on these particular examples without demonstrating general performance on unseen messages.

The rule-based baseline also performed well on the selected categories because its keyword rules aligned with the examples. Its performance may be weaker when messages use unfamiliar language, describe multiple issues, or communicate a hazard indirectly.

The evaluation focused on issue-category accuracy, urgency accuracy, and JSON validity. It did not independently measure the accuracy of sentiment, recommended teams, escalation explanations, or data-sensitivity assessments. Further testing on a larger and more diverse dataset is needed before considering operational deployment.

## Responsible AI and Security Considerations

A practical deployment should include:

- **Human oversight:** HIGH and CRITICAL reports should be reviewed promptly under an approved escalation procedure.
- **Privacy protection:** minimise or redact personal information before sending messages to an external model.
- **Secure credentials:** store API keys in a secrets manager or environment configuration rather than hardcoding them or committing them to GitHub.
- **Output validation:** validate JSON structure, required fields, and permitted category and urgency values.
- **Prompt-injection resilience:** treat employee messages as untrusted data and test for attempts to manipulate classification.
- **Error handling:** handle API failures, invalid responses, and unavailable services safely.
- **Monitoring:** record appropriate model and prompt versions, monitor classification errors, and regularly evaluate new examples.
- **Fairness and accessibility:** test different communication styles and ensure reports from different employees can be interpreted consistently.

The prototype demonstrates a potential approach to ESG message triage. It does not establish that an automated escalation or incident-management system has been deployed.

## Repository Contents

The repository includes the Colab notebook and experiment evidence generated during development.

Key result files include:

- `full_eight_case_comparison.csv` — comparison of the original prompt and baseline.
- `llm_vs_baseline.csv` — LLM versus rule-based results.
- `rule_based_baseline.csv` — baseline classification results.
- `refined_prompt_results.csv` — results for Refined Prompt V1.
- `refined_raw_outputs.json` — recorded raw outputs from V1.
- `v2_prompt_results.csv` — results for Refined Prompt V2.
- `v2_raw_outputs.json` — recorded raw outputs from V2.
- `prompt_version_comparison.csv` — consolidated comparison of the approaches.
- `llm_outputs.json` — saved original LLM output data.
- `Q3_ESG_Triage_Evidence.zip` — archive of the experiment evidence, if included in the repository.

Filenames may vary depending on the final repository organisation.

## Reproducing the Experiment

1. Open `BUS5001_Q3_ESG_Message_Triage.ipynb` in Google Colab.
2. Install the required Google Gen AI Python SDK and dependencies using the notebook's setup cells.
3. Configure the `GEMINI_API_KEY` secret in Colab and grant the notebook access to it.
4. Run the notebook cells in order to initialise the client, define the test cases, execute the classifiers, and evaluate the results.
5. Review the generated CSV and JSON files.

A valid Gemini API key and API access are required to rerun the LLM experiments. API availability and model behaviour may change over time, so reruns may not reproduce identical outputs.

**Security note:** Never commit your API key, credentials, or real employee reports containing sensitive information to this repository.

## Conclusion

This experiment demonstrates how a structured LLM workflow and iterative prompt engineering can support ESG message classification and urgency assessment. Refined Prompt V2 achieved 100% category and urgency accuracy on the eight predefined test cases, while the rule-based baseline achieved 100% category accuracy and 75% urgency accuracy.

These findings are specific to the experimental dataset. A real deployment would require broader evaluation, schema validation, privacy safeguards, human oversight, and monitoring before the system could be relied upon for operational decisions.
