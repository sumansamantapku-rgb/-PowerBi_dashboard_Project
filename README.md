# -PowerBi_dashboard_Project
This is PowerBi end to end project


🎧 Spotify Power BI Dashboard Analysis
✨ Project Overview

This repository contains the source data, Power BI report file, and documentation for a comprehensive data analysis of Spotify listening habits and music characteristics. The project utilizes Power BI to transform and visualize data, providing insights into popular tracks, artist performance, and key audio features (e.g., danceability, energy, acousticness).

📊 Dashboard Features
The final Power BI dashboard includes several interactive visualizations and key performance indicators (KPIs), such as:

Top Artists and Tracks: Ranking of the most streamed artists and songs over the analyzed period.

Audio Feature Analysis: Distribution and correlation of features like Danceability, Energy, and Valence across the dataset.

Genre Insights: Breakdown of listening habits by music genre.

Time Series Trends: Analysis of track popularity and feature changes over time (if applicable to the dataset).

Interactive Filters: Slicers for filtering data by Artist, Track, Genre, and Year.

⚙️ Technology Stack
The following tools and technologies were used to create this project:
Tool,Purpose
Power BI Desktop,"Primary tool for data import, transformation (Power Query), data modeling, and visualization."
Python / R (Optional),"Used for initial data cleaning, manipulation, or scraping (if applicable)."
SQL (Optional),Used for initial data extraction or storage (if applicable).
GitHub,Version control and project hosting.


📁 Repository StructureThe project files are organized as follows:

├── data/
│   ├── spotify_data_raw.csv         # Raw, unprocessed data file
│   └── spotify_data_cleaned.csv     # Final, cleaned dataset used for the report
├── documentation/
│   └── data_dictionary.md           # Explanation of columns and data sources
├── images/
│   └── dashboard_preview.png        # Screenshot of the final dashboard 
├── Spotify_Analysis_Report.pbix     # The main Power BI Desktop file
└── README.md                        # This file




🚀 Getting StartedTo view or interact with the dashboard, you have two main options:
1. Viewing the Live Report (Recommended)The published report can be viewed directly on the Power BI Service via this link:
(https://app.powerbi.com/links/9M-mlfHgQq?ctid=c181540e-95e2-408b-aead-cdef017d0b5f&pbi_source=linkShare&bookmarkGuid=43dd951e-99d0-4cda-9274-2d51d53c69ee)

2. Exploring LocallyTo open and explore the report in Power BI Desktop:
   https://github.com/sumansamantapku-rgb/-PowerBi_dashboard_Project
   
3.Download Power BI Desktop: Ensure you have the latest version installed.

4.Open the file: Double-click the spotify_Top_50 World

Data Source Check: Power BI may prompt you to refresh the data. The data sources are configured to look at the .csv files in the data/ folder.


🔍 Data Source
The data used in this analysis was sourced from google gemini


🔍DAX measurements which are used by the following data source:
DEFINE 
    // 🔹 Popularity Metrics
    MEASURE 'Top-50-world'[Average Popularity] = AVERAGE('Top-50-world'[popularity])
    MEASURE 'Top-50-world'[Max Popularity] = MAX('Top-50-world'[popularity])
    MEASURE 'Top-50-world'[Min Popularity] = MIN('Top-50-world'[popularity])

    // 🔹 Position Metrics
    MEASURE 'Top-50-world'[Average Chart Position] = AVERAGE('Top-50-world'[position])
    MEASURE 'Top-50-world'[Best Chart Position] = MIN('Top-50-world'[position])
    MEASURE 'Top-50-world'[Worst Chart Position] = MAX('Top-50-world'[position])
    MEASURE 'Top-50-world'[Number of #1 Hits] = 
        CALCULATE(
            COUNTROWS('Top-50-world'),
            'Top-50-world'[position] = 1
        )

    // 🔹 Song & Artist Counts
    MEASURE 'Top-50-world'[Total Songs] = DISTINCTCOUNT('Top-50-world'[song])
    MEASURE 'Top-50-world'[Total Artists] = DISTINCTCOUNT('Top-50-world'[artist])
    MEASURE 'Top-50-world'[Average Songs per Artist] = 
        DIVIDE([Total Songs], [Total Artists])

    // 🔹 Duration Metrics
    MEASURE 'Top-50-world'[Average Song Duration (min)] = 
        DIVIDE(AVERAGE('Top-50-world'[duration_ms]), 60000)
    MEASURE 'Top-50-world'[Shortest Song (min)] = 
        DIVIDE(MIN('Top-50-world'[duration_ms]), 60000)
    MEASURE 'Top-50-world'[Longest Song (min)] = 
        DIVIDE(MAX('Top-50-world'[duration_ms]), 60000)

    // 🔹 Album Insights
    MEASURE 'Top-50-world'[Average Album Tracks] = AVERAGE('Top-50-world'[total_tracks])
    MEASURE 'Top-50-world'[Singles Count] = 
        CALCULATE(COUNTROWS('Top-50-world'), 'Top-50-world'[album_type] = "single")
    MEASURE 'Top-50-world'[Albums Count] = 
        CALCULATE(COUNTROWS('Top-50-world'), 'Top-50-world'[album_type] = "album")

    // 🔹 Explicit Content
    MEASURE 'Top-50-world'[Explicit Songs] = 
        CALCULATE(COUNTROWS('Top-50-world'), 'Top-50-world'[is_explicit] = TRUE())
    MEASURE 'Top-50-world'[Non-Explicit Songs] = 
        CALCULATE(COUNTROWS('Top-50-world'), 'Top-50-world'[is_explicit] = FALSE())
    MEASURE 'Top-50-world'[Explicit Song %] = 
        DIVIDE([Explicit Songs], [Total Songs])

// 🔹 Release & Recency (Fixed)
MEASURE 'Top-50-world'[Average Release Year] =
    AVERAGEX('Top-50-world', YEAR('Top-50-world'[release_date]))

MEASURE 'Top-50-world'[Most Recent Release Year] =
    MAXX('Top-50-world', YEAR('Top-50-world'[release_date]))

MEASURE 'Top-50-world'[Oldest Release Year] =
    MINX('Top-50-world', YEAR('Top-50-world'[release_date]))


    // 🔹 Chart Dynamics
    MEASURE 'Top-50-world'[Total Chart Entries] = COUNTROWS('Top-50-world')
    MEASURE 'Top-50-world'[Unique Chart Dates] = DISTINCTCOUNT('Top-50-world'[date])
    MEASURE 'Top-50-world'[Average Songs per Chart Date] = 
        DIVIDE([Total Chart Entries], [Unique Chart Dates])
    MEASURE 'Top-50-world'[Average Popularity Trend per Date] = 
        AVERAGEX(VALUES('Top-50-world'[date]), [Average Popularity])
        
EVALUATE
    SUMMARIZECOLUMNS(
        "Average Popularity", [Average Popularity],
        "Max Popularity", [Max Popularity],
        "Min Popularity", [Min Popularity],
        "Average Chart Position", [Average Chart Position],
        "Best Chart Position", [Best Chart Position],
        "Worst Chart Position", [Worst Chart Position],
        "Number of #1 Hits", [Number of #1 Hits],
        "Total Songs", [Total Songs],
        "Total Artists", [Total Artists],
        "Average Songs per Artist", [Average Songs per Artist],
        "Average Song Duration (min)", [Average Song Duration (min)],
        "Shortest Song (min)", [Shortest Song (min)],
        "Longest Song (min)", [Longest Song (min)],
        "Average Album Tracks", [Average Album Tracks],
        "Singles Count", [Singles Count],
        "Albums Count", [Albums Count],
        "Explicit Songs", [Explicit Songs],
        "Non-Explicit Songs", [Non-Explicit Songs],
        "Explicit Song %", [Explicit Song %],
        "Average Release Year", [Average Release Year],
        "Most Recent Release Year", [Most Recent Release Year],
        "Oldest Release Year", [Oldest Release Year],
        "Total Chart Entries", [Total Chart Entries],
        "Unique Chart Dates", [Unique Chart Dates],
        "Average Songs per Chart Date", [Average Songs per Chart Date],
        "Average Popularity Trend per Date", [Average Popularity Trend per Date]
    )



🤝 Contribution
If you find any issues or have suggestions for improvements, feel free to open an issue or submit a pull request!






