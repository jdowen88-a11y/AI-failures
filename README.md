# AI Failures / ICU Defensive Monitor

This repo contains a defensive detection-training core for matching incoming telemetry against **real-world incident and campaign fixtures**.

The point is simple: a real safety monitor should not learn from cartoon hacker soup. It needs real campaigns, real ATT&CK mappings, real behavior, real telemetry shape, and real expected detections.

## What this repo includes

- Real-world campaign records for known public incidents and active/recent threat activity.
- Safe telemetry fixtures that let the bot recognize behavior without carrying runnable malicious material.
- A scoring engine that compares incoming events against real cases.
- Safety validation that blocks runnable exploit/malware payload fields from entering the fixture set.
- Tests proving the detector can identify real-world behavior patterns.

## What this repo intentionally does not include

This repo does **not** include live malware, working exploit payloads, web shells, ransomware samples, credential theft code, destructive commands, or copy/paste attack chains.

That is not a toy limitation. It is the boundary that keeps a defensive project from becoming a weapon cache with a polite folder name.

Instead, this repo stores the **detection-relevant shape** of the activity:

- Source type
- ATT&CK techniques
- Telemetry features
- Behavioral markers
- Expected result
- Severity
- Source reference
- Safety notes

## Install

```bash
python -m pip install -e .
```

## Run the demo

```bash
icu-score --events icu/data/sample_events.jsonl --cases icu/data/real_world_cases.json
```

## Example Python use

```python
from icu.engine import load_cases, score_event

cases = load_cases("icu/data/real_world_cases.json")

event = {
    "source_type": "endpoint_edr",
    "telemetry_features": [
        "native_windows_admin_tools_used",
        "remote_discovery_activity",
        "commands_blend_with_admin_activity",
        "low_malware_artifact_presence"
    ],
    "mitre_techniques": ["T1059", "T1047", "T1018"]
}

print(score_event(event, cases))
```

## Folder layout

```text
.
├── icu/
│   ├── cli.py
│   ├── engine.py
│   ├── safety.py
│   └── data/
│       ├── real_world_cases.json
│       └── sample_events.jsonl
├── tests/
│   ├── test_engine.py
│   └── test_safety.py
├── docs/
│   ├── ADDING_CASES.md
│   └── SAFETY_BOUNDARY.md
└── pyproject.toml
```

## Operating rule

Real examples are stored as **defensive telemetry fixtures**.

Runnable payloads are blocked at ingestion.

That lets the bot recognize actual hostile activity without preserving material that can be copied into an attack.
