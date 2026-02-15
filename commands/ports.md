---
description: List processes listening on ports
allowed-tools: Bash(whport:*)
---

List all processes listening on ports using whport.

!`whport list 2>&1 || echo "whport not installed — brew install lu-zhengda/tap/whport"`

Analyze the output above. Flag any unexpected listeners on common ports (80, 443, 3000, 5432, 6379, 8080, 27017). For each listener, explain what the process likely is. Offer to get more details with `whport info <port>` or kill with `whport kill <port>`.
