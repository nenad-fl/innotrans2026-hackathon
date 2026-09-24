# InnoTrans 2026 Hackathon

This repository contains the public materials, datasets, reference code, and participant documentation for the Alstom InnoTrans 2026 Hackathon challenge - **Alstom Intelligence: Talk To My Train**.

The challenge is to build a conversational AI agent that could help an urban operator, such as Berlin U-Bahn, understand, anticipate, and respond to passenger-flow changes caused by events, disruptions, and weather. A strong solution should produce answers that are relevant, explainable, grounded in the available data, and useful in an operational setting.

## Challenge

The agent should help answer questions such as:

- Which stations are likely to become overcrowded after a line suspension?
- How might a major concert or public event change passenger flows?
- Which stations are most at risk during an event combined with adverse weather?
- What actions or rerouting options should an operator consider?

Solutions are evaluated on relevance, reliability, stress testing, innovation, and real-world impact. See the [problem statement](docs/innotrans2026_hackathon_problem_statement.md) for the complete challenge description and evaluation criteria.

## Repository Contents

| Directory or file | Description |
| --- | --- |
| `src/` | Used by participants to upload their agents, MCP servers, data-building scripts, and notebooks. |
| `data/` | Training and testing data available for solution development. |
| `docs/` | Public problem statement, schema, and hackathon questions. |

## Dataset

The dataset describes a partial Berlin U-Bahn network and covers different urban data from **June 10, 2026 through September 21, 2026** for the training period.

### Main data sources

- **Network topology:** stations, served lines, station coordinates, and bidirectional connections.
- **Passenger flows:** simulated station-level counts at 15-minute intervals.
- **Events:** Berlin concerts, matches, conferences, public gatherings, and other events with time, location, category, and estimated attendance.
- **Closures:** simulated disruption scenarios, including affected times and descriptions.
- **Weather:** temperature, humidity, precipitation, wind, pressure, cloud cover, and weather-condition codes.
- **Energy:** simulated daily energy consumption by U-Bahn line.

Read the complete [dataset schema](docs/dataset_schema.md) before using the data. Important relationships include:

- `stations_with_ubahn.station_id` is the station key.
- Flow station columns match `stations_with_ubahn.station_name`.
- `flows.timestamp` joins to the weather timestamp at the 15-minute grain.
- Events do not contain station IDs and may need to be associated with stations through their venue or address.

## Getting Started

### 1. Clone the repository

```bash
git clone <repository-url>
cd <repository-directory>
```

### 2. Create a Python environment

Python 3.10 or newer is recommended.

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install pandas networkx numpy openai fastmcp mcp openpyxl python-dotenv
```

## Security: LLM Endpoints and Credentials

This is a public repository. Never commit or push sensitive configuration, including:

- API keys, access tokens, passwords, or connection strings.
- Private LLM endpoint URLs, internal hostnames, or IP addresses.
- `.env` files or other files containing credentials.
- Secrets exposed in notebook outputs, logs, screenshots, tracebacks, or API responses.

Store sensitive values locally in a `.env` file inside your team folder. The repository's `.gitignore` excludes `.env` files.

```dotenv
LLM_BASE_URL=https://your-private-endpoint.example.com/v1
LLM_API_KEY=replace-with-your-secret-key
LLM_MODEL=replace-with-your-model-name
```

Commit only a safe `.env.example` file containing empty values or obvious placeholders:

```dotenv
LLM_BASE_URL=
LLM_API_KEY=
LLM_MODEL=
```

Load the values from the environment instead of hardcoding them in source code:

```python
import os

from dotenv import load_dotenv
from openai import OpenAI

load_dotenv()

client = OpenAI(
    base_url=os.environ["LLM_BASE_URL"],
    api_key=os.environ["LLM_API_KEY"],
)
model = os.environ["LLM_MODEL"]
```

Before committing or pushing, clear notebook outputs and inspect exactly what Git will publish:

```bash
git check-ignore -v src/TEAM_X/.env
git status --short
git diff --cached
```

Stage only the intended team folder or individual files. Avoid `git add .`.

```bash
git add src/TEAM_X
git diff --cached
```

If a secret is committed or pushed, treat it as compromised. Revoke or rotate it immediately and notify the repository organizer. Deleting it in a later commit does not remove it from Git history.

## Building a Solution

Recommended capabilities include:

1. Load and validate the network, flow, event, closure, weather, and energy data.
2. Normalize timestamps and document all assumptions about local time and time windows.
3. Map events and disruptions to affected stations and lines.
4. Combine network topology with flow history to identify likely rerouting and pressure points.
5. Ground every answer in calculations or retrieved records and communicate uncertainty clearly.
6. Keep the interface usable for an operator working under time pressure.

The [training questions](docs/hackathon_questions_training.md) provide representative workflows for developing and testing a solution.

## Data and Evaluation Rules

- Each team will be evaluated using the same set of questions and the [evaluation criteria](docs/ALSTOM_Challenge_Evaluation_Onepager.pdf).
- Treat the data as simulated or partial where the schema says so; do not infer unsupported passenger-level origin-destination journeys.

## Documentation

- [Problem statement](docs/innotrans2026_hackathon_problem_statement.md)
- [Dataset schema](docs/dataset_schema.md)
- [Training questions](docs/hackathon_questions_training.md)

## License and Usage

This repository is provided for the InnoTrans 2026 Hackathon. Follow the event organizers' instructions for permitted use, redistribution, data handling, and publication of solutions.