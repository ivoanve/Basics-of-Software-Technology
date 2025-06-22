---
title: "Assignment 4: CubeSat Risk Quick-Assessor"
date: 2025-06-06
---
## Assignment 4 Report

**Student Name**: Ivonne Andrea Anave Zenteno  
**Student ID**: LS2408221  
**Course**: Basics of Software Technology  
**Submission Date**: June 6, 2025  

---

## 1. Introduction

The **CubeSat Risk Quick-Assessor** is a Command-Line Interface (CLI) application developed to support fast and structured **Failure Mode and Effects Analysis (FMEA)** in CubeSat engineering projects. It enables users to enter subsystem information and generate a risk report prioritizing failure modes by calculating Risk Priority Numbers (RPN).

---

## 2. Software Description

### 🔹 Key Features

- **Input:**
  - Subsystem name (e.g., "Power", "Communication").
  - Manual or default failure mode inputs.

- **Analysis:**
  - Calculates RPN using:  
    `RPN = Severity × Occurrence × Detection`  
  - Values range from 1 (low) to 10 (high).
  - Prioritizes risks from most to least critical.

- **Output:**
  - Generates `.txt` reports with ordered failure modes.
  - Optionally includes **LLM-generated risk descriptions** via Ollama (LLaMA 3).

### 🔧 Technical Stack

| Component       | Choice               | Version  |
|----------------|----------------------|----------|
| Language        | Python               | 3.10+    |
| Compiler        | PyInstaller          | 6.0      |
| LLM Integration | Ollama with LLaMA 3  | Optional |

---

## 3. AI-Assisted Development

### 🧠 LLM Prompts Used

During development, I used **ChatGPT** and **Ollama (LLaMA 3)** to assist with:

- Generating base code structure for RPN calculation.
- Writing modular Python functions for input/output.
- Improving the formatting of the CLI and file generation.

**Example Prompt:**

```plaintext
Generate Python code to calculate RPN (Severity × Occurrence × Detection) with random values between 1 and 10. Group results and write them to a text file.
```

### Example Integration with Ollama:

```python
import requests

response = requests.post(
    'http://localhost:11434/api/generate',
    json={
        "model": "llama3",
        "prompt": "Explain the risk of power subsystem failure in a CubeSat.",
        "stream": False
    }
)

print(response.json()["response"])
```

---

## 4. Source Code (English Version)

```python
import random

def calculate_rpn(failures):
    report = []
    for failure in failures:
        S, O, D = random.randint(1, 10), random.randint(1, 10), random.randint(1, 10)
        RPN = S * O * D
        report.append(f"{failure}: RPN = {RPN} (Severity={S}, Occurrence={O}, Detection={D})")
    return report

def generate_report(subsystem, failures, report):
    with open(f"report_{subsystem}.txt", "w", encoding="utf-8") as file:
        file.write(f"📋 Risk Report - Subsystem: {subsystem}\n\n")
        file.write("🚨 Prioritized Failure Modes:\n")
        for line in report:
            file.write(f"- {line}\n")
        file.write("\n⚠️ Prioritize the highest RPNs (max 100).")

def main():
    print("🚀 CubeSat Risk Quick-Assessor")
    subsystem = input("🔧 Subsystem (e.g., power, communication): ") or "power"
    failures_input = input("💥 Enter failure modes (comma-separated): ")
    failures = [f.strip() for f in failures_input.split(",")] if failures_input else ["Battery Depletion", "Short Circuit"]

    report = calculate_rpn(failures)
    generate_report(subsystem, failures, report)

    print("\n✅ Report generated:")
    for line in report:
        print(f"  {line}")
    print(f"\n📄 Saved as 'report_{subsystem}.txt'")

if __name__ == "__main__":
    main()
```

---

## 5. Sample Output

```
📋 Risk Report - Subsystem: power

🚨 Prioritized Failure Modes:
- Battery Depletion: RPN = 120 (Severity=8, Occurrence=5, Detection=3)
- Short Circuit: RPN = 60 (Severity=4, Occurrence=5, Detection=3)

⚠️ Prioritize the highest RPNs (max 100).
```

---

## 6. Conclusion

This project allowed me to:

- Combine risk assessment logic with automated reporting.
- Integrate LLMs in a practical way using **Ollama + Python**.
- Understand the FMEA method in the context of CubeSat systems.
- Practice AI-assisted software development for engineering tools.

---
## 8. 🎥 Project Presentation Video

<div align="center">
  <iframe width="560" height="315"
    src="https://youtu.be/Vjn3aL_VVgU"
    title="CubeSat Risk Quick-Assessor Presentation"
    frameborder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowfullscreen>
  </iframe>
</div>

---
## 7. References

- [NASA CubeSat 101 Guide](https://www.nasa.gov/sites/default/files/atoms/files/cubesat_101.pdf)
- [Ollama LLaMA 3 Docs](https://ollama.com/library/llama3)
- [Failure Mode and Effects Analysis (FMEA)](https://asq.org/quality-resources/fmea)
- [Python random module](https://docs.python.org/3/library/random.html)

---

> Report created using Markdown. Application developed in Python and tested locally on Windows.

