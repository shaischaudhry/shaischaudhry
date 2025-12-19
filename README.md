<div align="center">
  <img src="./assets/header.svg" width="100%" alt="Shais Chaudhry" />
</div>

<p align="center">
  <a href="https://linkedin.com/in/shaischaudhry"><img src="https://img.shields.io/badge/LinkedIn-0A66C2?style=flat-square&logo=linkedin&logoColor=white" alt="LinkedIn"/></a>
  <a href="mailto:shais.rehman.cs@gmail.com"><img src="https://img.shields.io/badge/Email-24292F?style=flat-square&logo=maildotru&logoColor=white" alt="Email"/></a>
</p>

Senior AI engineer, five years in, mostly remote with US teams.

My work tends to sit between machine learning and backend infrastructure. Computer vision models, retrieval systems, and the services that have to keep them alive once they leave the notebook. Healthcare and industrial platforms for the most part, which is a nice way of saying the output has to be right and somebody will ask you why it wasn't.

I do my own deployment. AWS, Azure and GCP, containers, IaC, the boring parts.

Currently at Devsinc. Before that, four years at Devntech.

### Stack

**ML and vision** &nbsp;
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white)
![TensorFlow](https://img.shields.io/badge/TensorFlow-FF6F00?style=flat-square&logo=tensorflow&logoColor=white)
![Keras](https://img.shields.io/badge/Keras-D00000?style=flat-square&logo=keras&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=flat-square&logo=opencv&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=flat-square&logo=scikitlearn&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat-square&logo=numpy&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat-square&logo=pandas&logoColor=white)

**Cloud and infra** &nbsp;
![Azure](https://img.shields.io/badge/Azure-0078D4?style=flat-square&logo=microsoftazure&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-FF9900?style=flat-square&logo=amazonwebservices&logoColor=white)
![GCP](https://img.shields.io/badge/GCP-4285F4?style=flat-square&logo=googlecloud&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white)
![Terraform](https://img.shields.io/badge/Terraform-844FBA?style=flat-square&logo=terraform&logoColor=white)
![Actions](https://img.shields.io/badge/GitHub%20Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat-square&logo=linux&logoColor=black)

**Services and data** &nbsp;
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![Django](https://img.shields.io/badge/Django-092E20?style=flat-square&logo=django&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-5FA04E?style=flat-square&logo=nodedotjs&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Elasticsearch](https://img.shields.io/badge/Elasticsearch-005571?style=flat-square&logo=elasticsearch&logoColor=white)
![Neo4j](https://img.shields.io/badge/Neo4j-4581C3?style=flat-square&logo=neo4j&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-FF4438?style=flat-square&logo=redis&logoColor=white)

**Models** &nbsp;
![Whisper](https://img.shields.io/badge/Whisper-412991?style=flat-square&logo=openai&logoColor=white)
![Claude](https://img.shields.io/badge/Claude-D97757?style=flat-square&logo=anthropic&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=flat-square&logo=openai&logoColor=white)
![Gemini](https://img.shields.io/badge/Gemini-8E75B2?style=flat-square&logo=googlegemini&logoColor=white)
![Llama](https://img.shields.io/badge/Llama-0467DF?style=flat-square&logo=meta&logoColor=white)
![Ollama](https://img.shields.io/badge/Ollama-000000?style=flat-square&logo=ollama&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-1C3C3C?style=flat-square&logo=langchain&logoColor=white)

<details>
<summary>How I usually wire a model into a service</summary>

<br/>

```text
                      ┌───────────────────────────────────────┐
  request ──────────► │  durable workflow  (replayable, typed) │
                      └──────────────────┬────────────────────┘
                                         │
                ┌────────────────────────┴────────────────────────┐
                ▼                                                 ▼
    ┌───────────────────────┐                    ┌────────────────────────────┐
    │   DETERMINISTIC       │                    │        MODEL               │
    │  ───────────────────  │                    │  ────────────────────────  │
    │  scoring              │ ◄──── gate ─────── │  inference / generation    │
    │  thresholds           │                    │  behind one interface, so  │
    │  business rules       │                    │  the backend is swappable  │
    │  schema validation    │                    │                            │
    └──────────┬────────────┘                    └─────────────┬──────────────┘
               │                                               │
               │                              typed contract + repair loop
               │                                               │
               │                    ┌──────────────────────────┴──────────┐
               │                    │  retry → reprompt → cheaper model →  │
               │                    │  deterministic path → human queue    │
               │                    └──────────────────────────┬──────────┘
               ▼                                               ▼
    ┌──────────────────────────────────────────────────────────────────────┐
    │  traces: tokens, latency, cost per tenant, full call tree            │
    └──────────────────────────────────────────────────────────────────────┘
```

Two things I keep coming back to.

The model produces output, it never decides an outcome. Scoring and thresholds stay on the deterministic side of that wall, which is what makes the whole thing auditable when somebody asks how a result was reached.

A retry is step one of five, not a recovery strategy. Reprompt, fall back to something cheaper, fall back to a deterministic path, then put it in front of a human. Each tier logs why it fell through, so failure modes end up as data rather than anecdotes.

</details>

<br/>

<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/shaischaudhry/shaischaudhry/output/github-snake-dark.svg" />
    <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/shaischaudhry/shaischaudhry/output/github-snake.svg" />
    <img alt="contribution graph" src="https://raw.githubusercontent.com/shaischaudhry/shaischaudhry/output/github-snake.svg" width="100%" />
  </picture>
</div>
