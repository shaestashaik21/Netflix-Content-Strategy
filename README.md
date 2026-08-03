🎬 **Netflix Content Strategy Analysis — Exploratory Data Analysis (EDA)**

📌 Business Problem: Analyze the data and generate insights that could help Netflix in deciding which type of shows/movies to produce and how they can grow the business in different countries.

Netflix is one of the most popular media and video streaming platforms in the world. As of mid-2021, they have over 10,000 movies and TV shows on their platform and over 222 million subscribers globally.

With increasing competition in the streaming market, Netflix needs to make data-driven decisions on:
1. What type of content to produce
2. Which genres and ratings perform best
3. Which countries to focus on for growth
4. How viewing preferences have shifted over time
   
This project analyzes Netflix's entire content library to answer exactly these questions — using data, not assumptions.

---------------------------------------------------------------------------------------------------------------------------------------------------



🗂️ **Table of Contents**

1. About the Dataset
2. Tools & Technologies Used
3. Project Workflow
4. Step 1 — Problem Statement & Basic Metrics
5. Step 2 — Data Shape, Types & Missing Values
6. Step 3 — Non-Graphical Analysis
7. Step 4 — Visual Analysis
8. Step 5 — Missing Value & Outlier Check
9. Step 6 — Insights from Analysis
10. Step 7 — Business Insights
11. Step 8 — Recommendations
12. How to Run This Project
13. Author

---------------------------------------------------------------------------------------------------------------------------------------------------------

📊 **About the Dataset**

The dataset contains a list of all movies and TV shows available on Netflix, along with detailed metadata for each title.

```markdown
| Column       | Description |
|--------------|-------------|
| show_id      | Unique ID for every Movie / TV Show |
| type         | Identifier — Movie or TV Show |
| title        | Title of the content |
| director     | Director(s) of the Movie / Show |
| cast         | Actors involved |
| country      | Country where the content was produced |
| date_added   | Date it was added on Netflix |
| release_year | Actual release year (ranges from 1925 to 2021) |
| rating       | TV Rating — e.g., TV-MA, TV-14, PG-13, R |
| duration     | Total duration — minutes (Movies) or seasons (TV Shows) |
| listed_in    | Genre / Category |
| description  | Short synopsis of the title |
```


Dataset Size: 8,807 rows × 12 columns

Dataset Source: https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/000/940/original/netflix.csv

How to Download DataSet: After clicking on the above link, you can download the files by right-clicking on the page and clicking on "Save As", then naming the file as per your wish, with .csv as the extension.

------------------------------------------------------------------------------------------------------------------------------------------------------------------
🛠️ **Tools & Technologies Used**

```markdown
| Tool         | Purpose                                      |
|--------------|----------------------------------------------|
| Python 3     | Core programming language                    |
| Pandas       | Data loading, cleaning, and wrangling        |
| NumPy        | Numerical operations                         |
| Matplotlib   | Building plots and charts                    |
| Seaborn      | Statistical and aesthetic visualizations     |
| Google Colab | Cloud-based Jupyter notebook environment     |
```


-----------------------------------------------------------------------------------------------------------------------------------------------------------------

🔄 **Project Workflow**:

**Load Data → Explore → Clean → Preprocess → Visualize → Insights → Recommendations**

The project follows a structured EDA approach across 8 clearly defined steps, matching the evaluation criteria of the case study.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

**Step 1 — Problem Statement & Basic Metrics:**

**What was done**:
1. Loaded the Netflix CSV dataset using pd.read_csv()
2. Displayed the first and last few rows using df.head() and df.tail()
3. Confirmed the dataset has 8,807 rows and 12 columns
4. Identified two content types: Movies and TV Shows
5. Noted that the dataset spans content from the early 1940s to 2021


**Key Observation**:

The dataset is rich with metadata covering country of production, genre, rating, duration, and date added — all of which are useful for strategic content decisions.

--------------------------------------------------------------------------------------------------------------------------------------------------------------

**Step 2 — Data Shape, Types & Missing Values**

**What was done**:


1. Checked shape using df.shape → (8807, 12)
2. Checked data types using df.dtypes
3. Ran df.isnull().sum() to detect missing values
4. Ran df.describe(include='all') for a full statistical summary


**Data Types Found**:


1. release_year — integer (only numerical column)
2. All other 11 columns — object (text/string)


**Missing Values Detected**:
```markdown
| Column      | Missing Count | % Missing |
|-------------|---------------|------------|
| director    | 2,634         | ~30%       |
| cast        | 825           | ~9.4%      |
| country     | 831           | ~9.4%      |
| date_added  | 10            | ~0.11%     |
| rating      | 4             | ~0.05%     |
| duration    | 3             | ~0.03%     |
| All others  | 0             | 0%         |
```

**Observation**:

Missing values in director, cast, and country are expected — many older or regional titles have incomplete metadata. Missing values in date_added, rating, and duration are negligible. No imputation was performed as these do not affect the overall analysis significantly.

**Statistical Summary Highlights**:

1. release_year ranges from 1925 to 2021, mean ≈ 2014
2. Most common content type: Movie (6,131 out of 8,807)
3. Most common rating: TV-MA (3,207 titles)
4. Most common country: United States (2,818 titles)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------

**Step 3 — Non-Graphical Analysis: Value Counts & Unique Attributes**

**What was done**:


1. Used df['column'].value_counts() on key columns
2. Used df.nunique() to check number of unique values per column


**Content Type Breakdown**:

| Type    | Count        |
|---------|-------------|
| Movie   | 6,131 (~70%) |
| TV Show | 2,676 (~30%) |

**Top 5 Countries by Content Count (before unnesting)**:

| Country         | Count |
|----------------|------:|
| United States  | 2,818 |
| India          | 972   |
| United Kingdom | 419   |
| Japan          | 245   |
| South Korea    | 199   |

**Most Common Ratings**:

| Rating | Count | Audience              |
|---------|------:|------------------------|
| TV-MA   | 3,207 | Mature Adults (18+)    |
| TV-14   | 2,160 | Teenagers (14+)        |
| TV-PG   | 863   | Parental Guidance      |
| R       | 799   | Restricted             |
| PG-13   | 490   | Parents Cautioned      |

**Top Genres (listed_in)**:

| Genre                                 | Count |
|--------------------------------------|------:|
| Dramas, International Movies         | 362   |
| Documentaries                        | 359   |
| Stand-Up Comedy                      | 334   |
| Comedies, Dramas, International Movies | 274 |
| Kids' TV                             | 220   |


**Unique Value Counts**:

1. 4,528 unique directors
2. 7,692 unique cast entries
3. 748 unique countries
4. 514 unique genre combinations
5. 17 unique rating categories

**Observation**:

Movies significantly outnumber TV Shows. The US dominates content production. Netflix primarily targets teen and adult audiences. Dramas, International Movies, and Documentaries are the most common genres.

----------------------------------------------------------------------------------------------------------------------------------------------------------------


**Step 4 — Visual Analysis (Univariate & Bivariate)**

Pre-Processing Done Before Visualization

Before creating charts, the following pre-processing steps were performed:


1. Date Parsing — Converted `date_added` from string to datetime format and extracted `year_added` and `month_added` as new columns
2. Duration Extraction — Used regex (`str.extract(r'(\d+)')`) to extract numeric minutes from movie duration strings
3. Handling Missing Values — Filled `NaN` in `cast`, `director`, and `country` with `'Unknown'` to prevent errors during unnesting
4. Column Unnesting — Split and exploded the multi-valued columns using `.str.split(', ')` and `.explode()`:
   i. `country_df` — one row per country per title
   ii. `director_df` — one row per director per title
   iii. `cast_df` — one row per actor per title


This unnesting was critical — for example, a movie with 3 country co-productions appears 3 times in `country_df`, allowing accurate per-country frequency analysis.

------------------------------------------------------------------------------------------------------------------------------------------------------------------

**4.1 Univariate Analysis**

A) **Distribution of Release Year (Histogram + KDE)**


1. Content is heavily right-skewed toward recent years
2. Minimal content before 1990
3. Sharp spike from 2015 onwards — when Netflix scaled globally and began producing originals
4. Peak release years: 2017, 2018, 2019


B) **Distribution of Movie Duration (Histogram + KDE)**


1. Bell-shaped curve centered around 90–100 minutes
2. Most movies fall between 80 and 120 minutes
3. Very short films (<60 min) and very long films (>200 min) are rare
4. This reflects the standard feature-film format audiences prefer


C) **Count of Titles by Release Year (Countplot)**


1. Confirms the exponential growth in content after 2015
2. 2018 and 2019 are the highest content-addition years


D) **Top 10 Countries by Content Count (after unnesting)**


1. United States far ahead of all others
2. India is a clear second — showing Netflix's heavy investment in Bollywood
3. UK, Canada, France, Japan, Spain, South Korea, Germany follow
4. South Korea showing notable presence — hinting at the K-drama boom


E) **Top 10 Actors on Netflix (after unnesting)**


1. Indian actors dominate: Anupam Kher (highest), Shah Rukh Khan, Akshay Kumar, Naseeruddin Shah, Om Puri, Julie Tejwani, Rupa Bhimani
2. Japanese actor Takahiro Sakurai and Yuki Kaji also appear — indicating anime depth
3. This confirms Netflix's deep investment in Indian and Japanese content

--------------------------------------------------------------------------------------------------------------------------------------------------------------

**4.2 Bivariate Analysis**

A) **Boxplot — Movie Duration**


1. IQR (middle 50%) is approximately 85 to 115 minutes
2. Median movie duration ≈ 98–100 minutes
3. Several outliers above 250–300 minutes — these are documentaries or special productions
4. Outliers were retained as they represent valid, real content


B) **Movies vs TV Shows Over Years (Countplot with hue)**


1. Until 2015, TV Shows were very few compared to Movies
2. From 2016 onwards, TV Shows increased significantly
3. This shows Netflix's strategic pivot toward episodic, binge-worthy content
4. TV Shows drive subscriber retention better than one-time movies


C) **Country vs Content Type (Top 5 Countries)**


1. United States — Movies dominate heavily, small TV Show count
2. India — Almost entirely Movies (Bollywood influence)
3. United Kingdom — More balanced between Movies and TV Shows
4. Canada — Movies dominant
5. This reveals Netflix's US and India focus is primarily movie-driven


-----------------------------------------------------------------------------------------------------------------------------------------------------------------

**Step 5 — Missing Value & Outlier Check**

Missing Value Summary (post-preprocessing):

| Column                                 | Missing | %     |
|----------------------------------------|--------:|------:|
| date_added / year_added / month_added | 98      | 1.11% |
| rating                                 | 4       | 0.05% |
| duration                               | 3       | 0.03% |
| All others                             | 0       | 0%    |

Note: `director`, `cast`, and `country` missing values were filled with `'Unknown'` during preprocessing.

**Outlier Detection**:

***Release Year Boxplot***:


1. No significant outliers
2. A few very old titles (pre-1940s) appear as mild outliers on the lower end
3. These are valid classic films, not data errors — retained


***Movie Duration Boxplot***:


1. Clear outliers above ~250 minutes
2. These represent documentaries, director's cut versions, or special feature-length productions
3. All retained as they are legitimate content


**Decision**: No outlier treatment was applied. All values represent real content with valid business context.

---------------------------------------------------------------------------------------------------------------------------------------------------------------


**Step 6 — Insights Based on Non-Graphical & Visual Analysis**

***6.1 Range of Attributes***


1. Release years span 1925 to 2021 — Netflix has both classic films and brand new content
2. Movie durations range from under 60 minutes to over 300 minutes
3. TV Shows range from 1 season to 10+ seasons
4. 748 unique countries of production — truly a global platform


***6.2 Distribution of Variables & Relationships***


1. Content distribution is heavily skewed toward Movies (70% vs 30% TV Shows)
2. However, TV Shows are growing faster year-on-year since 2016 — a clear strategic shift
3. Content production is concentrated in a few countries — USA, India, and UK account for the majority
4. Genre distribution shows Drama, Comedy, and International content dominate across both content types

**6.3 Plot-by-Plot Comments**

| Plot                     | Observation |
|--------------------------|-------------|
| Release Year Histogram   | Sharp growth after 2015; Netflix's global push drove massive content addition |
| Movie Duration Histogram | 80–120 minutes is the sweet spot; audiences prefer standard feature length |
| Release Year Countplot   | 2017–2019 are peak years; post-2020 content dipped slightly (likely COVID impact) |
| Top 10 Countries Countplot | USA dominates; India is a fast-growing second market |
| Top 10 Actors Countplot  | Indian actors dominate; Japan also has strong representation |
| Movie Duration Boxplot   | Tight IQR around 90–100 min; outliers are valid long-form content |
| Content Over Years (hue) | TV Shows rising steeply from 2016; Movies still dominant but the gap is closing |
| Country vs Type          | USA and India are movie-heavy; UK has more TV Show balance |

--------------------------------------------------------------------------------------------------------------------------------------------------------

**Step 7 — Business Insights**


***These are patterns observed in the data along with what can be inferred from them.***



1. **Movies dominate, but TV Shows are the growth driver**

Movies make up 70% of Netflix's library, but TV Shows have grown much faster since 2016. TV Shows drive higher subscriber retention because viewers return episode after episode, and season after season. This trend suggests Netflix is already aware of this and is investing more in series.

2. **Netflix's content library exploded after 2015**

The number of titles added per year grew dramatically from 2015 onwards, peaking around 2017–2019. This coincides with Netflix's global expansion and its shift from a licensing model to producing original content. Netflix clearly scaled content to win international markets.

3. **The United States is the backbone — India is the growth engine**

The US contributes nearly 3x more content than any other country. But India is a strong second with 972 titles — this signals Netflix's heavy bet on the South Asian market. South Korea's presence is also growing rapidly, driven by the global popularity of K-dramas.

4. **Drama, Comedy, and International content are Netflix's bread and butter**

These genres dominate both movies and TV shows consistently. International Movies and Dramas appear most frequently. This tells us that emotionally relatable, story-driven content performs across cultures.

5. **Netflix is built for mature and teen audiences**
TV-MA (3,207 titles) and TV-14 (2,160 titles) together make up over 60% of all content. Children and family content exists but is a smaller share. Netflix's identity is bold, mature storytelling.

6. **Netflix prefers short-run TV series**

Most TV Shows on Netflix have 1–3 seasons. This is a deliberate strategy — launch a limited series, test audience response, then decide whether to renew. It reduces production risk while experimenting with diverse content.

7. **A small set of trusted creators appear repeatedly**
   
A few directors and actors show up across multiple titles (e.g., Rajiv Chilaka directed 19 titles, David Attenborough appeared in 19 productions). Netflix clearly builds long-term creative partnerships to ensure consistent quality and reduce risk.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------

**Step 8 — Recommendations**

***Simple, data-backed, actionable items for business executives. No technical jargon.***



1. **Invest more in TV Shows — especially limited series**

TV Shows create long-term viewer habits. A subscriber who is watching a 3-season drama is far less likely to cancel than someone who finishes a 2-hour movie. Netflix should increase its TV Show production, particularly limited series (1–3 seasons) that allow testing before renewal.

2. **Double down on India, South Korea, and Japan**
   
These three markets show strong and growing content presence. Netflix should invest in local-language original productions — with native writers, directors, and actors — to attract and retain subscribers in these high-growth regions. Indian films and K-dramas already have proven global crossover appeal.

3. **Focus content investment on Drama, Comedy, and International genres**

The data clearly shows these genres dominate. Netflix should continue developing content in these areas while experimenting within them — for example, crime dramas, romantic comedies, and documentary series — to stay relevant across demographics.

4. **Maintain focus on mature audiences, while slowly expanding family content**
   
TV-MA and TV-14 ratings make up the majority of the library. Netflix's core identity is bold, adult storytelling. However, gradually adding more family-friendly and children's content would help grow the subscriber base to include household accounts and parents.

5. **Time new content releases strategically**

Analysis of `date_added` shows certain months have higher content addition rates. Netflix should align major original releases with peak streaming periods to maximize viewership and buzz.

6. **Lock in successful creators with exclusive, long-term deals**

Directors and actors who appear repeatedly across Netflix titles are proven assets. Signing them to exclusive deals ensures a consistent content pipeline, reduces casting uncertainty, and builds audience loyalty to familiar faces and storytelling styles.

----------------------------------------------------------------------------------------------------------------------------------------------------------------

▶️ **How to Run This Project**

**Option 1 — Open in Google Colab (Recommended — No Setup Needed)**

**Collab Link**: https://colab.research.google.com/drive/1Mhy2abu-Z_1G_ddFkKPlxQQ0jT9DVQXY

(*Just click the above Link. The notebook will open in your browser. Click Runtime → Run All to execute everything*).

**Option 2 — Run Locally on Your Machine**

Step 1: Clone this repository
https://github.com/Dilawarsuraj/Netflix-Content-strategy-EDA

cd netflix-content-strategy-eda

Step 2: Install required Python libraries
pip install pandas numpy matplotlib seaborn

Step 3: Download the dataset
Visit the link below, right-click → Save As → save as netflix.csv
https://colab.research.google.com/drive/1Mhy2abu-Z_1G_ddFkKPlxQQ0jT9DVQXY

Step 4: Launch Jupyter Notebook
jupyter notebook DAV1_Project.ipynb

------------------------------------------------------------------------------------------------------------------------------------------------------------

👤 Author

**Dilawar Suraj Shaik**

**Data Analyst** | Python | EDA | Business Insights


🔗 LinkedIn: https://www.linkedin.com/in/dilawarsuraj/
💻 GitHub: https://github.com/Dilawarsuraj?tab=repositories
📧 Email: dilawarsuraj2@gmail.com

---------------------------------------------------------------------------------------------------------------------------------------------------------------

 DataSet Link: https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/000/940/original/netflix.csv
 
 Solution Link: https://colab.research.google.com/drive/1Mhy2abu-Z_1G_ddFkKPlxQQ0jT9DVQXY#scrollTo=WZT3WHmnNorY
 How to Download DataSet: *After clicking on the above link, you can download the files by right-clicking on the page and clicking on "Save As", then naming the file as per your wish, with .csv as the extension*

------------------------------------------------------------------------------------------------------------------------------------------------------------------

  Note: **If you found this project useful or interesting, consider giving it a star on GitHub!**
 ----------------------------------------------------------------------------------------------------------------------------------------------------------------
                                                                
                                                                Happy Learning : )
                                    
 






















