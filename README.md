#  2021 F1 Telemetry & Championship Analysis: Hamilton vs. Verstappen

![F1 Dashboard Preview](dashboard.png) 
## Project Overview
The 2021 Formula 1 season featured one of the tightest championship battles in the sport's history between Lewis Hamilton and Max Verstappen. This project leverages **Power BI** to move beyond basic points tracking and dive into the actual telemetry. By calculating the true, per-lap average racing pace and comparing it to cumulative championship points, this dashboard visually proves exactly how raw speed translated into the final standings.

## Technical Stack & Data Engineering
*   **Tool:** Power BI Desktop (DAX, Power Query)
*   **Data Modeling:** Star Schema relationship established between the core telemetry fact table (`2021 F1 Cleaned`) and the dimensional calendar table (`races`).
*   **Data Transformation:** Filtered raw millisecond race durations to isolate competitive racing conditions. Implemented **"Clean Air"** logic to remove outliers like DNFs (Did Not Finish) and extremely slow safety car laps, ensuring a highly accurate performance comparison.

## Key Features & Insights
1.  **Lap-by-Lap Pace Battle:** An inverted Y-axis line chart tracking the true average lap time (in seconds) for each driver across the 22-race calendar.
2.  **Cumulative Points Area Chart:** A running total visualization demonstrating the momentum shifts throughout the season.
3.  **Executive KPI Scoreboard:** Dynamic "Tale of the Tape" cards summarizing final points, total wins, and overall season average pace.

## Core DAX Measures Highlight

**1. True Average Racing Pace (Sec)**
Calculates the accurate per-lap speed by dividing the total race duration (converted to seconds) by the total laps completed, applying the filter context for competitive laps only.
```dax
Average Racing Pace(Sec) = 
AVERAGEX(
    FILTER('2021 F1 Cleaned', '2021 F1 Cleaned'[clean air flag] = "Clean"), 
    ('2021 F1 Cleaned'[milliseconds] / 1000) / '2021 F1 Cleaned'[laps]
).
```
**2. Cumulative Championship Points
Generates a running total of points that dynamically updates round-by-round to visualize the championship timeline.
```dax
Cumulative Points = 
CALCULATE(
    SUM('2021 F1 Cleaned'[points]),
    FILTER(
        ALL('races'),
        'races'[round] <= MAX('races'[round])
    )
)
```
