Interpretable Clinical Equation Discovery for Diabetes & Gallstone Disease

An experimental biomedical-informatics framework for discovering interpretable clinical risk equations from patient biomarkers using large language models (LLMs), symbolic methods, and statistical validation.

> **Research status:** Experimental research prototype. This repository is intended for scientific investigation and reproducibility, not clinical diagnosis, treatment, or medical decision-making.

Overview

Many clinical prediction systems prioritize predictive performance while producing models that are difficult to interpret physiologically. This project investigates whether LLMs can help generate compact mathematical relationships between routinely measured biomarkers and disease outcomes, while imposing explicit constraints on mathematical validity, physiological plausibility, interpretability, and statistical performance.

The current research direction focuses on diabetes and gallstone-related clinical prediction.

The core idea is a two-stage discovery and validation framework:

1. LLM-guided equation generation

● Provide an LLM with a defined set of candidate biomarkers and an allowed mathematical grammar.

● Generate candidate equations representing possible disease-risk relationships.

● Preserve the symbolic form of each candidate rather than treating the LLM as a black-box predictor.

2. Scientific and statistical validation

● Parse and verify generated equations.

● Reject mathematically invalid expressions.

● Test physiological constraints and expected response shapes.

● Evaluate predictive performance on clinical datasets.

● Compare discovered equations with conventional statistical baselines.

● Test whether discovered relationships remain reproducible across datasets.

The objective is not simply to find the equation with the highest score. The project treats interpretability, biological plausibility, robustness, and reproducibility as first-class requirements.

Research Question

> **Can LLM-guided symbolic equation discovery identify compact, physiologically plausible biomarker relationships for diabetes and gallstone-related outcomes that remain statistically useful when evaluated on independent data?**

This leads to several secondary questions:

● Can an LLM generate useful symbolic relationships rather than only conventional linear combinations?

● Can physiological constraints eliminate mathematically valid but biologically implausible equations?

● Can a compact equation achieve useful discrimination without becoming an opaque machine-learning model?

● Do discovered equations generalize beyond the dataset on which they were generated?

● Which mathematical structures repeatedly emerge across independent datasets or disease targets?

● How much does an equation’s predictive performance change when its biomarkers are perturbed or removed?

Framework

The intended pipeline is:

Clinical datasets

       │

       ▼

Data cleaning & preprocessing

       │

       ▼

Biomarker selection

       │

       ▼

LLM-guided symbolic equation generation

       │

       ▼

Expression parsing & mathematical validation

       │

       ▼

Physiological plausibility checks

       │

       ▼

Statistical evaluation

       │

       ├──────────────► Baseline models

       │

       ▼

Multi-objective candidate filtering

       │

       ▼

Independent / cross-dataset validation

       │

       ▼

Interpretability & robustness analysis

       │

       ▼

Final candidate equations

Candidate Equation Grammar

The discovery system can restrict LLM-generated expressions to a controlled mathematical grammar rather than allowing arbitrary code.

Candidate operations include:

● Addition: +

● Subtraction: -

● Multiplication: *

● Division: /

● Logarithm: log(x)

● Square root: sqrt(x)

● Square: square(x)

● Exponential: exp(x)

● Positive-part / threshold behavior: max(0, x - T)

● Sigmoid transformations

● Piecewise functions

● Ratios

● Interaction terms

This grammar is intentionally limited so that candidate equations remain inspectable and can be evaluated by deterministic software.

Example feature types

Depending on the disease and dataset, candidate variables may include quantities such as:

● Age

● BMI

● Blood pressure

● Lipid measurements

● Glucose-related measurements

● Diabetes status

● Kidney-function measurements

● Smoking status

● Sex

● Other clinically available biomarkers

The exact feature set should be defined by the dataset and study protocol rather than assumed to be universally applicable.

Physiological Constraints

A central component of the framework is separating mathematical validity from scientific plausibility.

An equation can be mathematically valid while still being biologically unreasonable. Candidate equations can therefore be evaluated against explicit constraints such as:

Threshold behavior

Some biological relationships may plausibly change after a physiological threshold.

max(0, x - T)

Saturation

A biological response may approach a plateau rather than increase indefinitely.

Sigmoidal behavior

Some relationships may be better represented by bounded transitions.

sigmoid(x)

U-shaped relationships

For some biomarkers, both unusually low and unusually high values may be associated with an outcome.

Ratios and interactions

Some mechanisms may depend on relationships between biomarkers rather than isolated measurements.

These rules should be treated as prespecified research constraints, not as proof that a particular equation represents a causal biological mechanism.

Validation Strategy

The project emphasizes validation beyond a single train/test split.

1. Internal validation

Candidate equations can initially be evaluated using resampling or held-out data from the development dataset.

Possible measures include:

● AUROC

● AUPRC

● Sensitivity

● Specificity

● Brier score

● Calibration

● Logistic-regression performance

● Confidence intervals

2. Independent validation

The strongest test of a discovered equation is evaluation on data that were not used during equation discovery or optimization.

The workflow should therefore maintain a strict separation between:

Discovery data

      ↓

Equation generation

      ↓

Equation selection

      ↓

Locked equation

      ↓

Independent validation data

Information from the validation set should not be used to modify the final equation.

3. Cross-dataset validation

Where compatible datasets are available, the same locked equation can be evaluated across independent populations.

This helps distinguish:

● dataset-specific associations,

● reproducible statistical relationships,

● and relationships that may be sensitive to population or measurement differences.

Baseline Comparisons

Discovered equations should be compared with established statistical baselines rather than evaluated in isolation.

Potential baselines include:

● Logistic regression

● Regularized logistic regression

● Individual-biomarker models

● Simple additive risk models

● Other appropriately sized interpretable models

The purpose of these comparisons is to determine whether symbolic discovery provides additional value in:

● discrimination,

● calibration,

● compactness,

● interpretability,

● robustness,

● or biological plausibility.

Multi-Objective Candidate Evaluation

A candidate equation should not be selected solely by predictive performance.

A conceptual evaluation function can combine:

Candidate quality

    ├── Predictive performance

    ├── Calibration

    ├── Generalization

    ├── Mathematical validity

    ├── Physiological plausibility

    ├── Complexity

    └── Interpretability

This creates a tradeoff between a highly complex expression that fits one dataset extremely well and a simpler expression that performs consistently across independent datasets.

Equation complexity can be quantified using measures such as:

● Number of operations

● Expression-tree depth

● Number of unique biomarkers

● Number of interaction terms

● Number of nonlinear transformations

Reproducibility

A major requirement of the project is that a discovered equation should be reproducible.

The research pipeline should record:

● Dataset version

● Feature definitions

● Preprocessing rules

● Missing-data handling

● Random seeds

● LLM model/version where available

● Prompt or generation configuration

● Candidate equations

● Validation metrics

● Rejection reasons

● Final selected equations

Generated equations should be stored in a machine-readable format so that the discovery process can be audited independently of the LLM.

Suggested Repository Organization

The exact repository structure should mirror the files actually committed to GitHub. A recommended organization is:

.

├── README.md

├── data/

│   ├── raw/

│   └── processed/

├── notebooks/

│   ├── exploration/

│   ├── equation_discovery/

│   └── validation/

├── src/

│   ├── data/

│   ├── equations/

│   ├── physiology/

│   ├── evaluation/

│   └── models/

├── prompts/

├── results/

│   ├── equations/

│   ├── metrics/

│   └── figures/

├── tests/

├── requirements.txt

└── LICENSE

This is an organizational template, not a claim that every directory currently exists.

Data

The repository should document the provenance and licensing/usage conditions of every dataset used.

For each dataset, record:

● Source

● Dataset version

● Population

● Outcome definition

● Feature definitions

● Inclusion/exclusion criteria

● Missing-data handling

● Preprocessing

● Whether the dataset was used for discovery, model selection, or final validation

Do not commit private patient information or restricted clinical data to the repository.

If a dataset cannot legally or ethically be redistributed, provide instructions for obtaining it from its original source rather than committing the raw data.

Statistical Considerations

Because this is a clinical prediction research project, several methodological risks require explicit attention.

Data leakage

Preprocessing parameters, feature selection, equation selection, and model tuning must not use information from held-out validation data.

Overfitting

LLM-generated equations can be highly flexible. A candidate that performs well on one dataset may simply encode dataset-specific noise.

Multiple comparisons

Generating many equations creates a large candidate search space. Validation procedures should account for the possibility that apparently strong candidates arise by chance.

Calibration

Discrimination alone does not establish that predicted probabilities are reliable.

External validity

A relationship discovered in one population may not transfer to another population because of differences in demographics, disease prevalence, clinical practice, measurement methods, or missingness.

Causal interpretation

A predictive equation is not automatically a causal model. Statistical association should not be presented as proof of biological causation.

Safety and Scope

This repository is a research and educational project.

The equations and models produced by this project:

● are not medical devices;

● are not validated for patient care;

● should not be used to diagnose disease;

● should not be used to determine treatment;

● should not replace evaluation by qualified healthcare professionals.

The purpose of the work is to investigate methods for interpretable biomedical prediction and scientific hypothesis generation.

Research Outputs

The repository can contain several classes of outputs:

Candidate equations

Machine-readable symbolic expressions generated during discovery.

Validation metrics

Performance and calibration measurements for each candidate.

Figures

Examples include:

● ROC curves

● Precision-recall curves

● Calibration curves

● Feature-response plots

● Equation-complexity comparisons

● Cross-dataset performance comparisons

● Robustness / perturbation analyses

Research documentation

Protocol descriptions, experiment notes, methodological decisions, and papers or reports associated with the project.

Reproducing an Experiment

A reproducible experiment should follow this general sequence:

1. Obtain the permitted dataset

2. Verify the dataset version

3. Run preprocessing

4. Define the biomarker and outcome specification

5. Generate candidate equations

6. Run mathematical validation

7. Run physiological constraint checks

8. Evaluate candidates on discovery data

9. Lock the selected candidate(s)

10. Evaluate on untouched validation data

11. Generate statistical reports and figures

12. Save configuration and results

The exact commands should be added here once the final repository entry points are fixed.

Project Status

Current stage: Research framework / prototype.

The project is designed to evolve from exploratory equation generation toward a rigorously validated symbolic clinical-prediction pipeline.

Future work can include:

● expanding disease-specific biomarker libraries;

● improving physiological constraint checking;

● testing additional independent datasets;

● evaluating equation stability across resampled populations;

● benchmarking different LLMs;

● studying whether the same mathematical relationships recur across datasets;

● improving automated auditability of generated equations.

Citation

If this repository is used in research or derived work, cite the associated paper or project report once the final publication metadata are available.

@misc{clinical_equation_discovery,

  title  = {Interpretable Clinical Equation Discovery for Diabetes and Gallstone Disease},

  author = {Mamidala, Pranav},

  year   = {2026},

  note   = {Research prototype}

}

License

Add the project’s actual license here once the repository licensing decision has been made.

Acknowledgments

This project builds on publicly available biomedical datasets, statistical methodology, and prior research in clinical prediction, symbolic regression, interpretable machine learning, and biomedical informatics.

All dataset-specific attribution and licensing requirements should be retained in the repository.

