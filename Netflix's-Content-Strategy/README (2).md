### Step 1: Setup - Importing Libraries

As always, we begin by importing our essential data science toolset,
including a new library for word clouds.

Code: import pandas as pd import numpy as np import matplotlib.pyplot as
plt import seaborn as sns from wordcloud import WordCloud

# Set a consistent style for our plots

sns.set_style('darkgrid')

### Step 2: Data Loading and Initial Inspection

We'll load the `netflix_titles.csv` dataset and perform a high-level
overview.

Code: !git clone "https://github.com/HarshvardhanSingh-13/Datasets"

Code: netflix_df = pd.read_csv('/content/Datasets/Netflix_Titles
Dataset/netflix_titles.csv') netflix_df.head()

Code: netflix_df.tail()

Code: netflix_df\['type'\].value_counts()

Code: netflix_df.tail()

Code: netflix_df\['director'\].value_counts()

Code: netflix_df.info()

Code: netflix_df.describe()

**Interpretation of `.info()`:** - We have 7787 entries (titles). -
**Key Problem:** The `date_added` column is of type `object` (a string),
not a `datetime` object. We cannot perform time-based analysis until
this is corrected. - **Missing Values:** `director`, `cast`, `country`,
`date_added`, and `rating` all have missing values. `director` has the
most significant number of nulls.

### Step 3: Data Cleaning and Transformation

This step is critical for ensuring our analysis is accurate. We will
handle missing values and correct data types.

#### **Theoretical Concept: Data Type Conversion & Handling Nulls**

Data often comes in non-ideal formats. Storing dates as strings, for
example, prevents us from extracting components like the year or month,
or from plotting data over time. Converting columns to their proper data
types (`pd.to_datetime`, `.astype()`) is a fundamental preprocessing
step.

For null values, we have several strategies: 1. **Drop:** If only a very
small percentage of rows have missing data, dropping them might be
acceptable (`.dropna()`). 2. **Fill/Impute:** Replace missing values
with a placeholder (like "Unknown") or a statistical measure (like the
mode for categorical data). This is useful when you don't want to lose
the other information in those rows.

Code: \# 1. Handle missing values in 'director' and 'cast' \# Since
these are text fields and many are missing, we'll fill them with
'Unknown'. netflix_df\['director'\] =
netflix_df\['director'\].fillna('Unknown') netflix_df\['cast'\] =
netflix_df\['cast'\].fillna('Unknown')

Code: netflix_df.isnull().sum()

Code: netflix_df\['country'\].value_counts()

Code: \# 2. Handle missing 'country' \# We'll fill with the mode, which
is the most common country. mode_country =
netflix_df\['country'\].mode()\[0\] netflix_df\['country'\] =
netflix_df\['country'\].fillna(mode_country)

Code: netflix_df.isnull().sum()

Code: \# 3. Drop the few rows with missing 'date_added' and 'rating' \#
Since the number is small (less than 0.2% of data), dropping them is a
safe option. netflix_df.dropna(subset=\['date_added', 'rating'\],
inplace=True)

Code: netflix_df.isnull().sum()

Code: \# 4. Convert 'date_added' to datetime objects \# Use
format='mixed' to handle potential variations in date formats
netflix_df\['date_added'\] = pd.to_datetime(netflix_df\['date_added'\],
format='mixed', dayfirst=False)

-   **format='mixed':** This argument tells pandas to infer the date
    format automatically. This is helpful when the date strings in the
    column have different formats.

-   **dayfirst=False:** This argument specifies that when the date
    format is ambiguous (e.g., 01/02/2023), it should be interpreted as
    month first (January 2nd) rather than day first (February 1st).

Code: \# 5. Create new features for year and month added
netflix_df\['year_added'\] = netflix_df\['date_added'\].dt.year
netflix_df\['month_added'\] = netflix_df\['date_added'\].dt.month

Code: netflix_df.head()

Code: \# Verify our cleaning and transformation print("Missing values
after cleaning:") print(netflix_df.isnull().sum())
print("`\nData `{=tex}types after transformation:")
print(netflix_df.dtypes)

## Exploratory Data Analysis & Visualization

#### 4.1 What is the distribution of content type?

Code: plt.figure(figsize=(8, 6)) type_counts =
netflix_df\['type'\].value_counts() plt.pie(type_counts,
labels=type_counts.index, autopct='%1.1f%%', startangle=140,
colors=\['#e60023', '#221f1f'\]) plt.title('Proportion of Movies vs. TV
Shows') plt.ylabel('') plt.show()

#### 4.2 How has content been added over time?

Code: \# Group data by year and content type content_over_time =
netflix_df.groupby(\['year_added', 'type'\]).size().unstack().fillna(0)

plt.figure(figsize=(14, 8)) content_over_time.plot(kind='line',
marker='o', figsize=(14, 8)) plt.title('Content Added to Netflix Over
the Years (by Type)') plt.xlabel('Year Added') plt.ylabel('Number of
Titles Added') plt.legend(title='Content Type') plt.grid(True)
plt.show()

**Insight:** By separating movies and TV shows, we can see that while
both grew significantly, the addition of movies accelerated much more
dramatically, peaking in 2019. The growth in TV shows has been more
steady. There appears to be a slight slowdown in content additions in
2020 and 2021, which could be due to the COVID-19 pandemic affecting
productions or the dataset being incomplete for the latest year.

Code: netflix_df.head(2)

#### 4.3 What are the most popular genres?

#### **Theoretical Concept: Handling Multi-Value Text Columns**

The `listed_in` column contains strings with multiple genres separated
by commas (e.g., "Dramas, International Movies"). To analyze each genre
individually, we need to transform the data. A common technique is
to: 1. **Split** the string in each row into a list of genres. 2.
**Explode** the DataFrame so that each genre in the list gets its own
row, duplicating the other information for that title. This allows us to
perform a `value_counts()` on the genres.

Code: \# Split the 'listed_in' column and explode it genres =
netflix_df.assign(genre=netflix_df\['listed_in'\].str.split(',')).explode('genre')

Code: \# Get the top 15 genres and their counts top_genres_counts =
genres\['genre'\].value_counts().reset_index() top_genres_counts.columns
= \['genre', 'count'\] \# Rename columns for clarity

# Select only the top 15 for plotting

top_genres_counts_plot = top_genres_counts.head(15)

plt.figure(figsize=(12, 8)) sns.barplot(y='genre', x='count',
data=top_genres_counts_plot, palette='mako', hue='genre', legend=False)
plt.title('Top 15 Genres on Netflix') plt.xlabel('Count')
plt.ylabel('Genre') plt.show()

#### 4.4 What is the distribution of content duration?

Code: \# Separate movies and TV shows movies_df =
netflix_df\[netflix_df\['type'\] == 'Movie'\].copy() tv_shows_df =
netflix_df\[netflix_df\['type'\] == 'TV Show'\].copy()

Code: \# Clean and convert duration for movies
movies_df\['duration_min'\] = movies_df\['duration'\].str.replace('
min', '').astype(int)

# Clean and convert duration for TV shows

tv_shows_df\['seasons'\] = tv_shows_df\['duration'\].str.replace('
Seasons', '').str.replace(' Season', '').astype(int)

Code: \# Plot the distributions fig, axes = plt.subplots(1, 2,
figsize=(18, 7))

# Movie Duration Distribution

sns.histplot(ax=axes\[0\], data=movies_df, x='duration_min', bins=50,
kde=True, color='skyblue').set_title('Movie Duration Distribution
(minutes)')

# TV Show Season Distribution

sns.countplot(ax=axes\[1\], x='seasons', data=tv_shows_df,
palette='rocket', order=tv_shows_df\['seasons'\].value_counts().index,
hue='seasons', legend=False).set_title('TV Show Season Distribution')

plt.show()

**Insight:** - The majority of movies on Netflix are between 80 and 120
minutes long, which is standard for feature films. - The vast majority
of TV shows on Netflix are short-lived, with most having only 1 season.
This could reflect a strategy of producing many pilots and only renewing
the most successful ones, or a focus on limited series.

#### 4.5 Where does the content come from? (Geographical Analysis)

Code: \# Handle the multi-country listings similar to genres countries =
netflix_df.assign(country=netflix_df\['country'\].str.split(',')).explode('country')

Code: \# Get the top 15 countries and their counts top_countries_counts
= countries\['country'\].value_counts().reset_index()
top_countries_counts.columns = \['country', 'count'\] \# Rename columns
for clarity

Code: \# Select only the top 15 for plotting top_countries_counts_plot =
top_countries_counts.head(15)

plt.figure(figsize=(12, 10)) sns.barplot(y='country', x='count',
data=top_countries_counts_plot, palette='viridis', hue='country',
legend=False) plt.title('Top 15 Content Producing Countries on Netflix')
plt.xlabel('Number of Titles') plt.ylabel('Country') plt.show()

**Insight:** The United States is by far the largest producer of content
available on Netflix. However, India is a very strong second, which
explains why so many of the top actors were from India. The UK, Japan,
and South Korea also represent major content markets for the platform,
emphasizing its global nature.

Code: netflix_df.head(2)

#### 4.6 What are the maturity ratings of the content?

Code: plt.figure(figsize=(12, 8)) sns.countplot(x='rating',
data=netflix_df, order=netflix_df\['rating'\].value_counts().index,
palette='crest', hue='rating', legend=False) plt.title('Distribution of
Content Ratings on Netflix') plt.xlabel('Rating') plt.ylabel('Count')
plt.xticks(rotation=45) plt.show()

**Insight:** A large portion of Netflix's content is aimed at mature
audiences, with `TV-MA` (Mature Audience) and `TV-14` (Parents Strongly
Cautioned) being the two most common ratings. This suggests a focus on
adult viewers over content for children (`TV-G`, `TV-Y`).

### Step 5: Feature Engineering - Content Freshness

Let's create a new feature to analyze how old content is when it gets
added to Netflix. This can tell us about their acquisition strategy
(buying old classics vs. releasing new originals).

Code: \# Create the 'age_on_netflix' feature
netflix_df\['age_on_netflix'\] = netflix_df\['year_added'\] -
netflix_df\['release_year'\]

# Filter out any potential errors where added_year is before release_year

content_age = netflix_df\[netflix_df\['age_on_netflix'\] \>= 0\]

plt.figure(figsize=(14, 7)) sns.histplot(data=content_age,
x='age_on_netflix', bins=50, kde=True) plt.title('Distribution of
Content Age When Added to Netflix') plt.xlabel('Content Age (Years)')
plt.ylabel('Number of Titles') plt.show()

**Insight:** The large spike at `0` indicates that a significant amount
of content is added in the same year it's released, which is
characteristic of "Netflix Originals." However, there is a very long
tail, showing that Netflix also heavily invests in acquiring licensed
content that can be decades old, building a deep library of classic
films and shows.

### Step 6: Deeper Multivariate Analysis

Code: \# Analyze movie duration across different top genres top_genres =
genres\['genre'\].value_counts().index\[:5\] genres_movies =
genres\[(genres\['type'\] == 'Movie') &
(genres\['genre'\].isin(top_genres))\].copy()
genres_movies\['duration_min'\] =
genres_movies\['duration'\].str.replace(' min', '').astype(int)

plt.figure(figsize=(15, 8)) sns.boxplot(data=genres_movies, x='genre',
y='duration_min', palette='pastel', hue='genre', legend=False)
plt.title('Movie Duration by Top Genres') plt.xlabel('Genre')
plt.ylabel('Duration (minutes)') plt.xticks(rotation=45) plt.show()

**Insight:** While the median duration for most top genres is similar
(around 90-100 minutes), we can see some interesting variations. For
example, Dramas tend to have a wider range of durations, with many
longer films. International Movies also show a broad distribution,
reflecting diverse filmmaking styles from around the world.

### Step 7: Word Cloud from Content Descriptions

As a final visual analysis, let's generate a word cloud from the
`description` column to see what themes and words are most common in
Netflix content.

Code: \# Combine all descriptions into a single string text = '
'.join(netflix_df\['description'\])

# Create and generate a word cloud image

wordcloud = WordCloud(width=800, height=400,
background_color='black').generate(text)

# Display the generated image

plt.figure(figsize=(15, 10)) plt.imshow(wordcloud,
interpolation='bilinear') plt.axis('off') plt.title('Most Common Words
in Netflix Content Descriptions', fontsize=20) plt.show()

**Insight:** The word cloud highlights common themes and subjects. Words
like "life," "family," "love," "young," "friends," and "world" are
prominent, suggesting that much of the content revolves around human
relationships and personal journeys. Action-oriented words like "find,"
"secret," and "new" also appear frequently.

### Step 8: Final Conclusion and Summary of Insights

This in-depth EDA of the Netflix dataset has revealed several key
characteristics and strategies of the platform's content library.

**Key Findings:** 1. **Content Strategy:** Netflix's library is
movie-heavy (\~70%), and the platform aggressively added content between
2016-2019. Their strategy involves a mix of brand new originals (added
the same year they are released) and a deep library of licensed older
content. 2. **Global Dominance:** While the US is the top content
producer, the platform is heavily international, with India being a
massive contributor. This is reflected in the top genres ("International
Movies") and most frequent actors. 3. **Target Audience:** The content
library is skewed towards mature audiences, with `TV-MA` and `TV-14`
being the most common ratings. 4. **Content Format & Genre:** Dramas and
Comedies are universally popular genres. Most movies stick to a standard
90-120 minute runtime, while the vast majority of TV shows only last for
a single season, suggesting a high-risk, high-reward approach to series
production. 5. **Common Themes:** Descriptions of content frequently
revolve around universal themes of life, family, love, and discovery.

**Limitations:** This dataset is a snapshot in time and lacks viewership
data. Therefore, our analysis is of the *supply* of content, not its
*demand* or popularity. Nonetheless, this EDA provides a strong,
multi-faceted understanding of the composition and evolution of the
Netflix library.

### Step 8: Final Conclusion and Summary of Insights

This in-depth EDA of the Netflix dataset has revealed several key
characteristics and strategies of the platform's content library.

**Key Findings:** 1. **Content Strategy:** Netflix's library is
movie-heavy (\~70%), and the platform aggressively added content between
2016-2019. Their strategy involves a mix of brand new originals (added
the same year they are released) and a deep library of licensed older
content. 2. **Global Dominance:** While the US is the top content
producer, the platform is heavily international, with India being a
massive contributor. This is reflected in the top genres ("International
Movies") and most frequent actors. 3. **Target Audience:** The content
library is skewed towards mature audiences, with `TV-MA` and `TV-14`
being the most common ratings. 4. **Content Format & Genre:** Dramas and
Comedies are universally popular genres. Most movies stick to a standard
90-120 minute runtime, while the vast majority of TV shows only last for
a single season, suggesting a high-risk, high-reward approach to series
production. 5. **Common Themes:** Descriptions of content frequently
revolve around universal themes of life, family, love, and discovery.

**Limitations:** This dataset is a snapshot in time and lacks viewership
data. Therefore, our analysis is of the *supply* of content, not its
*demand* or popularity. Nonetheless, this EDA provides a strong,
multi-faceted understanding of the composition and evolution of the
Netflix library.

Code:

Code:

Code:
