---
# For Jekyll front matter (optional, depends on theme)
title: "Power Outages: When the Lights Go Out"
author: Yi-Hsuan Kuo & Tony Zheng
---

# Power Outages: When the Lights Go Out  
**Analyzing patterns in electricity disruption severity**

---

## Introduction
![Power grid](assets/grid.jpg){: width="400"}  
Electricity outages impact millions annually. We analyzed 10 years of outage data to answer:  
🔍 **What factors make some outages last longer than others?**  
Our findings help utilities prioritize response efforts and improve infrastructure resilience.

---

## Data Cleaning and Exploratory Data Analysis
### Key Cleaning Steps
- Removed 12% incomplete duration records
- Classified outage start times as **Day (9AM-5PM)** or **Night**
- Created hurricane flag from storm name records

```python
# Sample cleaned data
print(outage[['STATE', 'DURATION', 'CAUSE']].head().to_markdown())
