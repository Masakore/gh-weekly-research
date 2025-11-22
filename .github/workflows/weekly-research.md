---
description: |
  This workflow monitors the sites listed below, summarizes the most important security
  news and updates, and publishes the findings in a new GitHub discussion for
  stakeholders to review.

on:
  schedule:
    # Every day, 10PM UTC
    - cron: "0 22 * * *"
  workflow_dispatch:

  stop-after: +1mo # workflow will no longer trigger after 1 month. Remove this and recompile to run indefinitely

permissions: read-all

network: 
  allowed:
    - defaults
    - "*.tavily.com"
    - "api.individual.githubcopilot.com"
    - "thehackernews.com"
    - "www.darkreading.com"
    - "www.scworld.com"

safe-outputs:
  create-discussion:
    title-prefix: "${{ github.workflow }}"
    category: "ideas"

tools:
  github:
    toolsets: [all]
  web-fetch:
  web-search:

mcp-servers:
  tavily:
    command: npx
    args: ["-y", "@tavily/mcp-server"]
    env:
      TAVILY_API_KEY: "${{ secrets.TAVILY_API_KEY }}"
    allowed: ["search", "search_news"]

timeout-minutes: 15

source: githubnext/agentics/workflows/weekly-research.md@d3422bf940923ef1d43db5559652b8e1e71869f3
---

# Weekly Research

## Job Description

Do a research investigation in the following websites and summarize the latest security news and updates past one week.

- https://thehackernews.com/
- https://www.darkreading.com/
- https://www.scworld.com/


Create a new GitHub discussion with title starting with "${{ github.workflow }}" containing a markdown report with

- A summary of the most important security news and updates found during the research

Only a new discussion should be created, no existing discussions should be adjusted.

At the end of the report list write a collapsed section with the following:
- All search queries (web, issues, pulls, content) you used
- All bash commands you executed
- All MCP tools you used
