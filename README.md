##  Indian Railways Station Network Analysis

Analysis of the operational hierarchy and geographic distribution of railway stations across India, based on official Indian Railways data (as on 01.12.2025).

# Overview

This project extracts, cleans, and analyzes zone/category-wise station data for Indian Railways, uncovering patterns in how stations are ranked, grouped, and spread across zones and divisions — and visualizes the network's operational structure.

# Data Source

List of Zone/Category-wise Railway Stations Opened for Passenger Services — Indian Railways, official PDF (as on 01.12.2025)

# Workflow
Extraction — Parsed tabular data from the source PDF using pdfplumber
Cleaning — Handled missing values, standardized text fields, resolved duplicate station codes/names using pandas
Feature Engineering — Derived fields: Category_Rank, Division_Station_Count, Zone_Share_%
Analysis — Zone-wise and division-wise aggregations to surface key patterns
Visualization — Bar charts and stacked bar charts using matplotlib and seaborn

# Tools
Excel
Python (Google Colab)
pdfplumber — PDF table extraction
pandas — data cleaning & analysis
matplotlib, seaborn — visualization

# Repository Contents
File	Description
Indian_Railways_network.ipynb	Full analysis notebook
Railway_station_zone-category_wise_list.pdf	Source data
Railway_station_zone-category_wise_list_CLEANED.csv	Cleaned, structured dataset
Indian_railways_report.docx	Summary report

# Key Insights
Distribution of stations by category (HG1–SG3) across zones
Top divisions by station count
Zones with the highest concentration of top-tier stations
States spanning multiple railway zones
