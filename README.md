Indian Railways Station Zone-Category Analysis 
Objective
This project set out to uncover how India's vast railway network is structured — how stations are ranked, grouped, and spread across zones and divisions — and to turn that raw, unstructured data into clear visual insights that tell a coherent story about the network's operational backbone..
Data Cleaning Report
1. Extraction: Tables were extracted page-by-page using pdfplumber.extract_tables(). Repeated per-page header rows were removed. Validation: the S No column ran 1–8,477 with zero gaps/duplicates, confirming no rows were lost across page breaks. Result: 8,477 rows extracted cleanly.

2. Missing Values: 3 rows had a blank District. These initially showed as "0 nulls" because the cells were empty strings, not true NaN — caught via manual inspection, not the standard isna() check. Labeled as "Unknown" rather than backfilled, to avoid introducing unverified external data.

3. Data Type Fix: S_No was stored as text instead of numeric, which silently broke row-lookup filters (.isin()). Converted to int.

4. Duplicate Station Codes: 36 duplicate Station Code values were found. Manual review classified them into three types:
•	Text inconsistencies (same station, inconsistent spelling/casing/spacing) — e.g. DARBANGA/DARBHANGA JN., SAHARSA/SAHARSA JN. → standardized text case, mapped known district-name variants, dropped the less-standard duplicate. 31 rows removed.
•	Genuine collisions (different real stations sharing a code) — e.g. BARA (Madhya Pradesh) vs BILARA (Rajasthan); KATTUR (Tamil Nadu) vs KOTTUR (Telangana) → kept both, since merging would misrepresent real stations.
•	Ambiguous cases (same code, different district, unclear if same station) — e.g. RAMGARHWA vs RAMGARHWA F → kept both, flagged rather than guessed.
5. Final Validation: 0 full-row duplicates, 0 nulls, all Station Category values valid (HG1–3, NSG1–6, SG1–3), 16 zones, 64 divisions confirmed.
Issue	Rows Affected	Resolution
Blank District (hidden as empty string)	3	Labeled "Unknown"
Incorrect S_No dtype	8,477	Cast to int
Duplicate station codes (text variants)	31	Dropped
Duplicate station codes (genuine/ambiguous)	10	Retained, flagged
Final dataset: 8,446 clean rows, ready for distribution analysis and visualization.

Feature Engineering
To move from raw categorical fields to quantifiable metrics, three derived columns were added. 
•	Category_Rank converts the text-based Station Category (HG1–3, NSG1–6, SG1–3) into a numeric hierarchy score from 1 (most important) to 12 (least important), enabling averaging and ranking rather than just counting categories. 
•	Division_Station_Count attaches the total number of stations in each station's Division as a per-row value, using groupby().transform('count'), making it directly usable for sorting and visualization without a separate lookup step. 
•	Zone_Share_% calculates what percentage of the national total (8,446 stations) each Zone accounts for, via value_counts(normalize=True), giving a direct, comparable measure of each Zone's geographic and operational footprint. All mapped values were validated with a null check (0 unmapped categories), confirming full coverage across the dataset.

Key Insights & Findings
1. Network Dominance and Share
•	Northern Railway (NR) Leadership: The Northern Railway (NR) zone holds the largest share of the network in this dataset at 12.02%. It also has the highest total number of stations (1,015).
•	Division-Level Analysis: At the divisional level, Lucknow is the largest by station count, managing 355 stations. It is followed by Nagpur (271) and Ferozpur (249).
2. Station Importance and Category Distribution
•	Top-Tier Concentration: The Eastern Railway (ER) has the highest concentration of top-tier stations based on average category rank (8.24). However, when looking specifically at the percentage of HG1-3 (top-tier) stations, the East Central Railway (ECR) leads with 38.21%, while the West Central Railway (WCR) has the lowest at 9.76%.
•	Division Importance: Danapur is identified as the most "important" division, having the lowest average category rank of 5.28. Conversely, the Mumbai division shows the highest average rank (9.42), indicating a higher concentration of lower-tier stations compared to its total count.
3. Geographical and State Connectivity
•	Central Connectivity Hub: Madhya Pradesh (MP) is the state spanning the most railway zones, with 6 different zones serving the state. This highlights MP's critical role as a central hub for national connectivity due to its geographic position.
•	Other Multi-Zone States: Uttar Pradesh, Rajasthan, and Maharashtra follow closely, each being served by 5 distinct zones.
Visualization Analysis
The analysis utilizes several key visualizations to communicate these findings:
•	Top 10 Divisions by Station Count: Lucknow division has the highest station count nationally (355), well ahead of Nagpur (271) and Ferozpur (249). Seven of the top ten divisions fall under just three zones (NR, ECR, NCR), showing station density is concentrated rather than evenly spread across the network.


•	Top-Tier Station Concentration by Zone: Measuring the % of HG1–HG3 (top-tier) stations per zone shows East Central Railway (ECR, 38.1%) and North Eastern Railway (NER, 37.8%) have the richest concentration of high-importance stations, while West Central Railway (WCR, 9.8%) has the lowest — despite WCR's states (Madhya Pradesh, Rajasthan) touching many zones. This shows zonal reach and station-quality concentration are independent attributes.

•	Zone Share of the National Network: Northern Railway (NR) holds the largest share of the network at 12.0% of all stations, consistent with its coverage of Delhi, Haryana, Uttar Pradesh, and Punjab. This single zone accounts for more stations than the three smallest zones (ECOR, SECR, WCR) combined.


Summary of Key Findings
•	Station count and station importance are distinct dimensions — a zone/division with many stations is not necessarily one with high-importance stations (e.g., WCR vs. ECR/NER).
•	Network density is geographically skewed toward Northern Railway and its associated divisions (Lucknow, Ferozpur, Delhi, Moradabad).
•	ECR and NER combine moderate size with the highest proportion of top-tier stations, marking them as strategically dense rather than simply large.
•	Central states like Madhya Pradesh act as multi-zone connective hubs, underscoring their structural role in national rail connectivity.
•	Metro-anchored divisions (Mumbai, Chennai Central, Sealdah, Howrah) have low average importance scores because major terminals are outnumbered by dense suburban halts.



Conclusion for Reporting
The data suggests a network that is heavily concentrated in the Northern and Eastern regions in terms of both quantity and station hierarchy. While large divisions like Lucknow drive station volume, divisions like Danapur are more "top-heavy" with high-category stations. Central India (Madhya Pradesh) serves as the primary cross-zonal bridge for the entire network.


Tools Used: Excel, Python (Google Colab) — pdfplumber for PDF table extraction; pandas for data cleaning, deduplication, and derived fields (Category_Rank, Division_Station_Count, Zone_Share_%); matplotlib and seaborn for visualizations.
