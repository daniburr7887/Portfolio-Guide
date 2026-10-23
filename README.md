# Danielle Davis | Business Intelligence Analyst

I build the dashboards, reports and data pipelines that operations teams use every day. Recently that has meant moving a company's Tableau reporting into Shiny apps (Python) and Slack-delivered reports: rebuilding the logic in code, checking every number against the original, and keeping it fast at production scale.

📫 [LinkedIn](https://www.linkedin.com/in/danielle-davis-560152142/)

***

## ⭐ Featured project

### [PBJ Staffing Explorer](https://github.com/daniburr7887/pbj-staffing-explorer) · [▶ Live dashboard](https://danielleburr.shinyapps.io/pbj-staffing-explorer/)

[![PBJ Staffing Explorer: state staffing table beside a county map of Texas](https://raw.githubusercontent.com/daniburr7887/pbj-staffing-explorer/main/docs/img/dashboard-states.png)](https://danielleburr.shinyapps.io/pbj-staffing-explorer/?tab=States)

How well staffed are U.S. nursing homes, and how much of that staffing comes from agency workers? This project turns CMS's public Payroll-Based Journal data (about 5M facility-day rows a year) into a full analytics stack.

- **Pipeline:** finds new quarterly releases automatically in the CMS data catalog and builds a DuckDB warehouse from layered SQL models (staging → dimension → fact → rollup)
- **Dashboard:** an R Shiny app with national trends, clickable county maps, a weekday-vs-weekend comparison, and a drill-down into any of ~14,600 facilities, with hover tooltips throughout
- **Tested:** testthat suite built on a hand-calculated fixture, covering the details that break silently: leading zeros in provider IDs, zero-census days, and weighted vs. naive averages
- **Built for deployment:** ships a 5 MB Parquet extract instead of the 385 MB warehouse
- **Documented:** a [design decision log](https://github.com/daniburr7887/pbj-staffing-explorer/blob/main/docs/decisions.md) explaining each choice and its trade-offs

`R` `Shiny` `ggiraph` `SQL` `DuckDB` `Parquet` `testthat`

***

## 📝 Case study

### [Moving a company's BI from Tableau to code](case-studies/tableau-to-shiny-migration.md)

How a small team moved ~70 Tableau dashboards and reports to Shiny for Python apps and an on-demand Slack report bot, and what it took to make the numbers match and keep things fast in production: hidden Tableau filters, time zones lost in replication, hour-long database lock pile-ups, and dropdowns with 40,000 options.

`SQL` `Redshift` `Shiny for Python` `Tableau` `Slack automation` `performance tuning`

***

## 🛠️ Skills

| Area | Tools |
|---|---|
| **Languages** | SQL, R, Python |
| **Dashboards & BI** | Shiny for Python, R Shiny (bslib, DT), Tableau, ggplot2 |
| **Data platforms** | Amazon Redshift, PostgreSQL, DuckDB |
| **Reporting automation** | Slack bots and scheduled reports, Excel exports |
| **Engineering practice** | Git/GitHub, code review, automated testing, Posit deployment |
| **AI-assisted development** | Claude Code for building, debugging and reviewing production code |

***

## 📂 Earlier projects

Course and self-directed analyses from when I started in analytics.

| Project | Tools | Description |
|---|---|---|
| [Bellabeat Fitness Tracker Case Study](https://github.com/daniburr7887/Google-Data-Analytics-Capstone) | R (tidyverse, ggplot2, lubridate) | Google Data Analytics capstone: analyzed fitness tracker usage patterns to inform a marketing strategy. |
| [AllTrails US Hiking Guide](https://github.com/daniburr7887/AllTrails-US-Hiking-Guide) · [Dashboard](https://public.tableau.com/views/USAlltrailsHikingGuide/Dashboard1?:language=en-US&:display_count=n&:origin=viz_share_link) | SQL, Tableau | Ranked top U.S. hikes by rating and features, and mapped the concentration of easy vs. difficult trails by state. |
| [COVID-19 & US International Travel](https://github.com/daniburr7887/COVID-19-Analysis) | SQL | Tracked cases, vaccinations and outbound international travel from 2019 to 2022 using joins, CTEs, temp tables and views. Inspired by [Alex The Analyst's COVID-19 project](https://www.youtube.com/watch?v=qfyynHBFOsM&list=PLUaB-1hjhk8H48Pj32z4GZgGWyylqv85f&index=1). |
