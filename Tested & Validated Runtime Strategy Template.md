# Proposal

**Proposal:** Standardizing "Tested & Verified" for \[RUNTIME NAME\] in Red Hat OpenShift AI

**1\. Executive Summary**

\[RUNTIME NAME\] is the industry standard for **\[KEY USE CASE/FRAMEWORK\]** model serving. Currently, RHOAI lists \[RUNTIME NAME\] as a "Tested and Verified" (T\&V) runtime. However, the lack of a formalized definition and a scoped testing strategy creates a grey area for our field teams and customers. This proposal defines exactly what "Tested and Verified" means, outlines a tiered testing strategy, and identifies the high-value open-source software (OSS) models we should validate to capture the majority of customer use cases, thereby boosting enterprise confidence without violating our out-of-the-box support boundaries.

**2\. The Problem Context**

RHOAI officially supports runtimes like vLLM, Caikit, and OpenVINO. While RHOAI supports the *ability* to add custom runtimes via the ServingRuntime Custom Resource Definition (CRD), it does not officially support the \[RUNTIME NAME\] container image itself.

However, because \[RUNTIME NAME\] is critical for customers leveraging hardware accelerators (**\[ACCELERATOR TYPE, e.g., Nvidia GPUs, Intel CPUs\]**) for **\[KEY WORKLOAD TYPES, e.g., non-LLM and multi-framework\]** workloads (**\[FRAMEWORK LIST\]**), we must provide a structured baseline of trust. If we do not define what works, customers will assume either *everything* works (creating support escalations) or *nothing* works (causing adoption friction).

**3\. Proposed Definition: "Tested and Verified" (T\&V)**

We must clearly demarcate the support boundary. For RHOAI, **Tested and Verified** for \[RUNTIME NAME\] should be defined as:

*Red Hat guarantees the interoperability, lifecycle management, and secure integration of the \[RUNTIME NAME\] Inference Server within the RHOAI KServe ecosystem. Red Hat verifies that \[RUNTIME NAME\] can successfully mount storage (PVC/S3), bind to hardware accelerators via the **\[ACCELERATOR OPERATOR NAME, e.g., Nvidia GPU Operator\]**, and expose metrics to OpenShift monitoring. Red Hat does not provide bug fixes, SLAs, or code-level support for the upstream \[RUNTIME NAME\] runtime itself.*

**4\. Testing Strategy & Scope**

To back up the T\&V claim, our internal team should execute a tiered testing matrix for every stable RHOAI release.

**Tier 1: Infrastructure & Lifecycle (The "Must Haves")**

* **Deployment:** Successful creation of the **\[RUNTIME NAME\]** ServingRuntime via the RHOAI Dashboard and GitOps (ArgoCD).  
* **Storage Integration:** Successful model fetching from S3-compatible storage (data connections) and Persistent Volume Claims (PVCs).  
* **Hardware Binding:** Verification that the **\[RUNTIME NAME\]** pod successfully requests and binds to **\[ACCELERATOR TYPE\]** via the OpenShift **\[ACCELERATOR OPERATOR NAME\]**.

**Tier 2: Core Serving & Networking**

* **Protocol Accessibility:** Validation of both **\[PRIMARY PROTOCOL, e.g., REST (HTTP port XXXX)\]** and **\[SECONDARY PROTOCOL, e.g., gRPC (port YYYY)\]** endpoints via KServe routing.  
* **Scaling:** Verification of KServe Serverless autoscaling (scale to zero, scale up on load) with the **\[RUNTIME NAME\]** runtime.

**Tier 3: Advanced Differentiators (The "Enterprise Edge")**

* **\[RUNTIME SPECIFIC CORE FEATURE 1, e.g., Dynamic Batching\]:** Testing **\[RUNTIME NAME\]**'s core differentiator—**\[DESCRIPTION OF FEATURE 1, e.g., grouping inference requests dynamically to maximize hardware throughput\]**.  
* **Telemetry:** Verification that **\[RUNTIME NAME\]**'s /metrics endpoint (**\[PORT NUMBER\]**) is successfully scraped by OpenShift monitoring and visible in Grafana dashboards.

**5\. Recommended Model Categories & OSS Models**

To test runtime effectively, we must use models that reflect the 80/20 rule for our customer base. We should focus on the frameworks where \[RUNTIME NAME\] excels: **\[FRAMEWORK LIST\]**.

**A. \[CATEGORY 1, e.g., Computer Vision\]**

\[RUNTIME NAME\] is the undisputed leader for vision models.

* **Model:** **\[MODEL 1.1\]** and **\[MODEL 1.2\]**.  
* **Framework to Test:** **\[FRAMEWORK A\]** and **\[FRAMEWORK B\]**.  
* **Why:** These are the foundational architectures for **\[KEY USE CASES\]**.

**B. \[CATEGORY 2, e.g., Natural Language Processing / Traditional ML\]**

Many enterprises are heavily invested in pre-LLM architectures for embeddings and text classification.

* **Model:** **\[MODEL 2.1\]**  
* **Framework to Test:** **\[FRAMEWORK C\]** and **\[FRAMEWORK D\]**.  
* **Why:** Used extensively for **\[KEY USE CASES\]**.

**C. \[CATEGORY 3, e.g., Tabular Data & Fraud Detection\]**

* **Model:** **\[MODEL 3.1\]**.  
* **Framework to Test:** **\[SPECIFIC BACKEND/LIBRARY, e.g., FIL backend\]** in **\[RUNTIME NAME\]**.  
* **Why:** Financial institutions migrating from legacy architectures rely heavily on **\[MODEL 3.1\]** for **\[KEY USE CASES\]**.

**D. \[CATEGORY 4, e.g., Generative AI / Large Language Models (LLMs)\]**

* **Model:** **\[MODEL 4.1\]** & **\[MODEL 4.2\]**.  
* **Framework to Test:** **\[RUNTIME NAME\]** with the **\[BACKEND A\]** or **\[BACKEND B\]** backend.  
* **Why:** While RHOAI provides a native **\[ALTERNATE RHOAI RUNTIME\]** runtime, customers heavily entrenched in the **\[VENDOR NAME\]** software ecosystem will want to serve LLMs through **\[RUNTIME NAME\]** to maintain a single, unified inference control plane.

\*Note: **\[CATEGORY 4\]** testing requires testing the specific **\[SPECIFIC CONTAINER VARIANTS\]** container variants, as **\[VENDOR NAME\]** has bifurcated containers to reduce bloat.

**6\. \[RUNTIME NAME\] Release Frequency & Verification of Latest Stable Version**

To define a testing strategy, we must align our QE cycles with **\[VENDOR NAME\]**’s release cadence.

* **Standard Release Cycle:** **\[VENDOR NAME\]** releases **\[RUNTIME NAME\]** open-source containers **\[FREQUENCY, e.g., monthly\]**.  
* **Enterprise Release Cycle (Strategic Target):** For enterprise stability, **\[ENTERPRISE PRODUCT NAME\]** releases a **\[BRANCH NAME\]** every **\[FREQUENCY, e.g., 6 months\]**.  
* **Verification of Proposed Models:** We must verify that all recommended models and frameworks are fully supported in the latest stable release, but with an important architectural caveat for our testing strategy: **\[ARCHITECTURE CAVEAT, e.g., specific backends require specialized containers\]**.

**7\. Additional Feature Tests (Recommended/Stakeholder Review)**

If QE bandwidth allows, adding the following tests will significantly elevate Red Hat’s authority in the AI serving space:

* **\[FEATURE 1, e.g., Token Streaming/Decoupled Mode\]:** Crucial for **\[WORKLOAD TYPE, e.g., GenAI\]**. We must verify that **\[RUNTIME NAME\]** can **\[VERIFICATION GOAL, e.g., maintain a persistent connection to stream responses\]** back to the user interface via KServe.  
* **\[FEATURE 2, e.g., Concurrent Model Execution\]:** Testing **\[RUNTIME NAME\]**'s ability to host multiple heterogeneous models simultaneously on a single **\[HARDWARE TYPE\]**, maximizing hardware utilization.  
* **\[FEATURE 3 \- Profiling/Tuning\]:** Validating that **\[VENDOR NAME\]**'s native CLI profiling tools (**\[TOOL NAME\]**) function correctly from within an OpenShift pod to help customers find the optimal **\[CONFIGURATION SETTINGS, e.g., batch size and memory configuration\]**.

**8\. Estimated Infrastructure Cost per Test Cycle**

To secure buy-in, we need to forecast the cloud infrastructure costs per RHOAI release. Assuming a hybrid testing approach (automated pipelines \+ targeted manual validation) requiring roughly **\[TOTAL HOURS\]** hours of active compute time per release, here is the AWS On-Demand baseline estimate:

| Hardware Tier (AWS Instance) | Use Case Tested | Hourly Rate (Est.) | Hours per Cycle | Cost per Cycle |
| ----- | ----- | ----- | ----- | ----- |
| \[HARDWARE 1 NAME\] | \[USE CASE 1\] | \[RATE 1\] | \[HOURS 1\] | \[COST 1\] |
| \[HARDWARE 2 NAME\] | \[USE CASE 2\] | \[RATE 2\] | \[HOURS 2\] | \[COST 2\] |
| \[HARDWARE 3 NAME\] | \[USE CASE 3\] | \[RATE 3\] | \[HOURS 3\] | \[COST 3\] |
| **Total Estimated Cloud Cost per RHOAI Release Cycle** |  |  |  | **\[TOTAL COST\]** |

**9\. Alignment with Red Hat's AI Strategy & Business Value**

1. **Frictionless On-Ramping:** By publishing a verified list of model architectures (**\[MODEL ARCHITECTURES\]**) that work seamlessly on **\[RUNTIME NAME\]** within RHOAI, we reduce the time-to-value for customers migrating legacy ML workloads to OpenShift.  
2. **Ecosystem Play:** This strategy strengthens our partnership with **\[VENDOR NAME\]**. It signals to the market that while Red Hat champions open-source (e.g., vLLM), we pragmatically embrace industry-standard enterprise tools (**\[RUNTIME NAME\]**).  
3. **Deflection of Support Overhead:** By clearly documenting *what* is verified (specific backends and models), support teams can easily deflect issues related to unsupported esoteric models or custom **\[RUNTIME NAME\]** backends back to the community or **\[VENDOR NAME\]**, protecting Red Hat's operating margins.

**10\. Next Steps for Execution**

1. **Pin the Version:** We should officially pin the "Tested and Verified" tag for the upcoming RHOAI release to the **\[STABLE ENTERPRISE BRANCH\]** branch (or the stable **\[STABLE OPEN-SOURCE BRANCH\]** open-source branch if the enterprise option is out of scope).  
2. **Publish a Support Matrix:** We publish a Red Hat Knowledgebase (KCS) article clearly mapping the Tier 1 models (**\[MODEL LIST\]**) to the specific **\[RUNTIME NAME\]** container tags we validated.

# Future Investigation & Investment

To turn the observed trends into a concrete engineering and product strategy for Red Hat OpenShift AI (RHOAI), we need to establish specific initiatives for our Quality Engineering (QE) and product management teams.

Here is how we should execute against each trend:

**1\. Strategy for \[TREND 1 NAME, e.g., Vendor's New Microservices\]**

**\[NEW PRODUCT NAME\]** are rapidly becoming **\[VENDOR NAME\]**'s preferred distribution method for enterprise AI. Because **\[NEW PRODUCT NAME\]** containers often use **\[RUNTIME NAME\]** under the hood, standardizing our **\[RUNTIME NAME\]** integration is step one. Step two is proving **\[NEW PRODUCT NAME\]** works seamlessly on RHOAI.

* **Actionable Recommendation:** Initiate a dedicated SPIKE specifically for deploying **\[NEW PRODUCT NAME\]** containers via KServe in RHOAI.  
* **Execution Details:**  
  * **Registry Integration:** Test and document the process of securely pulling **\[NEW PRODUCT NAME\]** containers from the **\[VENDOR REGISTRY\]** registry using OpenShift secrets linked to the KServe ServingRuntime.  
  * **API Key Injection:** **\[NEW PRODUCT NAME\]** often requires an **\[KEY ENVIRONMENT VARIABLE\]** environment variable to initialize. We need to verify that KServe allows secure, seamless injection of this variable at deployment time.  
* **Target Deliverable:** A published Reference Architecture showing a popular **\[NEW PRODUCT NAME\]** deployed on RHOAI, proving that our **\[RUNTIME NAME\]** foundational work translates directly to **\[NEW PRODUCT NAME\]** compatibility.

**2\. Strategy for \[TREND 2 NAME, e.g., Agentic AI & Ensemble Models\]**

As customers build agents, they need to run multiple models in sequence. Passing this data back and forth between the client and server adds massive latency. **\[RUNTIME NAME\]** solves this natively with **\[ADVANCED CAPABILITY, e.g., Ensemble Models\]**, which processes the entire pipeline on the server side.

* **Actionable Recommendation:** Promote **\[RUNTIME NAME\]** "**\[ADVANCED CAPABILITY\]**" to a Tier 2 testing requirement in the next fiscal half.  
* **Execution Details:**  
  * **Pipeline Testing:** QE should build a test case featuring a simple inference pipeline (e.g., Python backend script that sanitizes text, automatically feeding into a PyTorch NLP model).  
  * **Documentation:** Red Hat can add immense value by providing pre-configured, tested YAML templates for common **\[ADVANCED CAPABILITY\]** patterns in our documentation.

**3\. Strategy for FIPS Compliance**

Public sector and highly regulated industries are massive revenue drivers for OpenShift. A FIPS-enabled OpenShift cluster loses its compliance edge if the workloads running on it are not compliant.

* **Actionable Recommendation:** Mandate FIPS-compliant testing in the CI/CD pipeline for the **\[VENDOR ENTERPRISE PRODUCT\]** integration.  
* **Execution Details:**  
  * **Environment:** QE must spin up at least one RHOAI test environment on a strictly FIPS-enabled OpenShift cluster.  
  * **Image Targeting:** Explicitly pull the **\[OS BASE\]** STIG/FIPS-ready **\[RUNTIME NAME\]** images provided by **\[VENDOR ENTERPRISE PRODUCT\]**, not the standard open-source images.  
* **Target Deliverable:** An official certification or capability statement confirming that RHOAI can maintain an end-to-end FIPS-compliant boundary when serving models via the **\[VENDOR ENTERPRISE PRODUCT\]** runtime.

**Title:** Support Scope for "Tested & Verified" Runtimes (\[RUNTIME NAME\] Inference Server) in Red Hat OpenShift AI

**Environment:** Red Hat OpenShift AI (RHOAI) Self-Managed and Cloud Service, KServe, ModelMesh, \[RUNTIME NAME\] Inference Server**Issue**

* What is the exact level of Red Hat support for the **\[RUNTIME NAME\]** Inference Server when deployed as a "Tested and Verified" runtime in Red Hat OpenShift AI?  
* How does a "Tested and Verified" runtime differ from a "Supported" or "Custom" runtime?  
* Who provides bug fixes or patches if **\[RUNTIME NAME\]** fails to serve a specific model?

**Resolution**

In Red Hat OpenShift AI (RHOAI), model-serving runtimes fall into three distinct tiers: **Supported**, **Tested and Verified**, and **Custom**. The **\[RUNTIME NAME\]** Inference Server is officially designated as a **Tested and Verified** runtime.**1\. The "Tested and Verified" Support Boundary**

For tested and verified runtimes, Red Hat focuses on validating that the community or third-party software operates reliably on the underlying Red Hat platform (OpenShift and RHOAI). Customers with an active RHOAI subscription **can open support tickets** regarding **\[RUNTIME NAME\]** deployments, provided the issue pertains to the Red Hat infrastructure.

**Red Hat provides support for:**

* **Platform Integration:** Successful deployment of the ServingRuntime and InferenceService Custom Resources (CRDs) via the RHOAI dashboard or OpenShift CLI.  
* **Networking & Routing:** Configuration of KServe passthrough routes for **\[PRIMARY PROTOCOL\]** or **\[SECONDARY PROTOCOL\]** endpoints.  
* **Storage & Infrastructure:** Mounting Persistent Volume Claims (PVCs) or S3-compatible data connections so **\[RUNTIME NAME\]** can pull model files.  
* **Hardware Acceleration:** Ensuring the **\[RUNTIME NAME\]** pod successfully binds to underlying hardware via the respective OpenShift Operators.  
* **Telemetry:** Ensuring OpenShift monitoring can successfully scrape the /metrics endpoint of the **\[RUNTIME NAME\]** server.

**Red Hat DOES NOT provide support for:**

* **Software Bugs & Code Fixes:** Red Hat does not maintain, distribute, or patch the upstream **\[RUNTIME NAME\]** container images.  
* **Model Failures:** Issues related to a model failing to load due to incompatible mathematical operations or errors internal to the runtime.  
* **Custom Backends:** Creating or compiling custom backends for **\[RUNTIME NAME\]**.

**2\. How this differs from "Custom Runtimes."**

If a user deploys a completely unverified **Custom Runtime**, Red Hat provides *zero* support for its configuration, maintenance, or integration.

By adhering to the "Tested and Verified" baseline for **\[RUNTIME NAME\]** (using the exact configurations provided in Red Hat documentation), customers receive Red Hat's backing for the platform integration layer, which they forfeit if using a purely custom runtime.**3\. Escalation Path & Resolution Matrix**

To ensure rapid resolution, please follow this escalation path depending on the nature of the error:

1. **RHOAI / OpenShift Platform Errors (e.g., Pods failing to schedule, KServe routing 503 errors, Dashboard UI errors):**  
   * Open a support case with **Red Hat Support**.  
2. **\[RUNTIME NAME\]** **Internal Errors (e.g., Core dumps within the container, specific models failing to compile, CVE vulnerabilities in the \[RUNTIME NAME\] base image):**  
   * Direct these inquiries to **\[VENDOR ENTERPRISE SUPPORT\]** (if using enterprise images) or file an issue in the upstream **\[RUNTIME NAME\]** **GitHub repository**.

# Draft Customer-Facing KCS Article

**Title:** Support Scope for "Tested & Verified" Runtimes (\[RUNTIME NAME\] Inference Server) in Red Hat OpenShift AI

**Environment:** Red Hat OpenShift AI (RHOAI) Self-Managed and Cloud Service, KServe, ModelMesh, \[RUNTIME NAME\] Inference Server Issue

* What is the exact level of Red Hat support for the **\[RUNTIME NAME\]** Inference Server when deployed as a "Tested and Verified" runtime in Red Hat OpenShift AI?  
* How does a "Tested and Verified" runtime differ from a "Supported" or "Custom" runtime?  
* Who provides bug fixes or patches if **\[RUNTIME NAME\]** fails to serve a specific model?

**Resolution**

In Red Hat OpenShift AI (RHOAI), model-serving runtimes fall into three distinct tiers: **Supported**, **Tested and Verified**, and **Custom**. The **\[RUNTIME NAME\]** Inference Server is officially designated as a **Tested and Verified** runtime. 

**1\. The "Tested and Verified" Support Boundary**

For tested and verified runtimes, Red Hat focuses on validating that the community or third-party software operates reliably on the underlying Red Hat platform (OpenShift and RHOAI). Customers with an active RHOAI subscription **can open support tickets** regarding **\[RUNTIME NAME\]** deployments, provided the issue pertains to the Red Hat infrastructure.

**Red Hat provides support for:**

* **Platform Integration:** Successful deployment of the ServingRuntime and InferenceService Custom Resources (CRDs) via the RHOAI dashboard or OpenShift CLI.  
* **Networking & Routing:** Configuration of KServe passthrough routes for **\[PRIMARY PROTOCOL\]** or **\[SECONDARY PROTOCOL\]** endpoints.  
* **Storage & Infrastructure:** Mounting Persistent Volume Claims (PVCs) or S3-compatible data connections so **\[RUNTIME NAME\]** can pull model files.  
* **Hardware Acceleration:** Ensuring the **\[RUNTIME NAME\]** pod successfully binds to underlying hardware via the respective OpenShift Operators.  
* **Telemetry:** Ensuring OpenShift monitoring can successfully scrape the /metrics endpoint of the **\[RUNTIME NAME\]** server.

**Red Hat DOES NOT provide support for:**

* **Software Bugs & Code Fixes:** Red Hat does not maintain, distribute, or patch the upstream **\[RUNTIME NAME\]** container images.  
* **Model Failures:** Issues related to a model failing to load due to incompatible mathematical operations or errors internal to the runtime.  
* **Custom Backends:** Creating or compiling custom backends for **\[RUNTIME NAME\]**.

**2\. How this differs from "Custom Runtimes."**

If a user deploys a completely unverified **Custom Runtime**, Red Hat provides *zero* support for its configuration, maintenance, or integration.

By adhering to the "Tested and Verified" baseline for **\[RUNTIME NAME\]** (using the exact configurations provided in Red Hat documentation), customers receive Red Hat's backing for the platform integration layer, which they forfeit if using a purely custom runtime. 

**3\. Escalation Path & Resolution Matrix**

To ensure rapid resolution, please follow this escalation path depending on the nature of the error:

1. **RHOAI / OpenShift Platform Errors (e.g., Pods failing to schedule, KServe routing 503 errors, Dashboard UI errors):**  
   * Open a support case with **Red Hat Support**.  
2. **\[RUNTIME NAME\]** **Internal Errors (e.g., Core dumps within the container, specific models failing to compile, CVE vulnerabilities in the \[RUNTIME NAME\] base image):**  
   * Direct these inquiries to **\[VENDOR ENTERPRISE SUPPORT\]** (if using enterprise images) or file an issue in the upstream **\[RUNTIME NAME\]** **GitHub repository**.

