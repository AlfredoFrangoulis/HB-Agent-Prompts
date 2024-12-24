# HIVE BRIDGE AI TRANSLATOR

> ## **`📢❗🚨 57.14285714285714% DONE IKUZOOOOO 🚨📢❗`**

## Tech Stack

- LLM
  - https://docs.mistral.ai/
  - (_MAYBE USING OPEN AI OR ANTHROPIC FOR PRODUCTION_)
- BACKEND
  - HZERO
- FRONTEND
  - https://hive-bridge.vercel.app/
  - NEXT JS (_TEMPORARY FOR DEVELOPMENT_)

## TODO

### LLM

- [x] JAVA
- [x] TS / JS / ECMASCRIPT
- [x] XML
- [ ] YML - ON PROGRESS 🚨
- [ ] SQL - ON PROGRESS 🚨
- [ ] VUE - ON PROGRESS 🚨
- [ ] Chinese Logic Agent Training Prompts 💀

### BACKEND

- [x] Agent Table to store agent (CRUD)
- [ ] FIX LOGIC FLOW (Move the translation request to the frontend) ((FOR PROD 💀))

### FRONTEND

- [x] CRUD AGENT PAGE (On Progress) - ON PROGRESS 🚨 gajadi 💀
- [x] CHANGE AGENT SELECTION LIST BY FETCHING





## KNOWN ISSUE

- If the LLM returned the response as code block (using backticks ```) it'll got stored too
  - Current solution 
    - Add prompt condition — e.g. `Ensure the line numbers in "details" align exactly with the original input file (do not add new line). Do not include markdown formatting (e.g., ```json) in the response. Return the JSON as plain text.`
    -  Sanitized the string
