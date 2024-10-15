## DATA 512 -  Homework 2: Considering Bias in Data

### Goal of the project
The goal of this assignment is to explore the concept of bias in data using Wikipedia articles. It will focus on articles about political figures from various countries, analyzing potential biases in the data obtained from internet sources like Wikipedia, particularly in relation to regional disparities or underrepresentation. The objective is to examine the correlation between the number of articles about politicians, the percentage of high-quality articles, and the population of each country. Through this analysis, we aim to identify any underlying biases in the data and address key questions, such as whether internet articles and content are reliable and unbiased sources for data analysis.

### License
This project is developed and distributed under the [MIT LICENSE](https://opensource.org/licenses/MIT), ensuring flexibility and openness for users and contributors. It allows anyone to use, modify, and distribute the code with minimal restrictions.
Under the MIT License, you are permitted to freely use the software for any purpose, but it comes with no warranty. The only requirement is that the original license and copyright notice must be included in any copies or substantial portions of the software.

### Process flow:
#### 1. Data Acquisition:
The data retrieval beings with two input files `politicians_by_country_AUG.2024.csv` which is obtained by scarping the Wikipedia [Category:Politicians by nationality](https://en.wikipedia.org/wiki/Category:Politicians_by_nationality)  to generate a list of Wikipedia article pages about politicians from a wide range of countries, and the `population_by_country_AUG.2024.csv` downloaded from the [world population data sheet](https://www.prb.org/international/indicator/population/table/) published by the Population Reference Bureau.

For each article in the raw data file, we get two fields, the *revision_id* and *article_quality*. The revision_id is obtained by calling the [Wikimedia API Endpoint](https://en.wikipedia.org/w/api.php). The article quality is fetched using a machine learning system called [ORES](https://www.mediawiki.org/wiki/ORES). This was originally an acronym for "Objective Revision Evaluation Service" but was simply renamed “ORES”. ORES is a machine learning tool that can provide estimates of Wikipedia article quality. The article quality estimates are, from best to worst:

FA - Featured article

GA - Good article (also known as A-Class)

B - B-Class article

C - C-Class article

Start - Start-class article

Stub - Stub-class article

For each article where ORES couldn't provide a quality score, or the revision ID was missing, we logged the article's title and revision ID in error_titles.txt. We also calculated and reported the error rate for rows with missing predictions. The error rate observed was **0.12 %**


#### 2. Data Cleaning and Merging:
Once the data was pulled from the API sources, the article title (politician name) and article quality we merged with the population data using **country** as the common key. We ensured consistency in country names, resolved duplicates. Any unmatched countries identified during the merging process were recorded and saved in the `wp_countries-no_match.txt` file.

#### 3. Metric calculation and analysis:

For each country and region, we calculated the following metrics:

- **Articles per capita:** Total number of articles about politicians divided by the country or region’s population (in millions of people).
- **High-quality articles per capita:** Number of high-quality (FA or GA) articles divided by the country or region’s population (in millions of people).

**Regional Aggregation**

We calculated these metrics not only for individual countries but also for geographic regions by aggregating the population and article data at the regional level.
<br>
<br>

These results are then sub-divided into 6 tables for detailed analysis (embedded in Jupyter Notebook)

- **Top 10 Countries by Coverage:** Countries with the highest articles per capita. <br>
- **Bottom 10 Countries by Coverage:** Countries with the lowest articles per capita.<br>
- **Top 10 Countries by High-Quality Articles:** Countries with the highest number of high-quality articles per capita.<br>
- **Bottom 10 Countries by High-Quality Articles:** Countries with the lowest number of high-quality articles per capita.<br>
- **Geographic Regions by Coverage:** Regions ranked by articles per capita.<br>
- **Geographic Regions by High-Quality Articles:** Regions ranked by high-quality articles per capita.


### Data and Code Sources
 - **Wikimedia API** : https://www.mediawiki.org/wiki/API:Info
 
    Wikimedia projects are free, collaborative repositories of knowledge, written and maintained by volunteers around the world. Each project hosts a different type of content, such as encyclopedia articles on Wikipedia and dictionary entries on Wiktionary. The Analytics API gives you open access to data about Wikimedia projects, including page views, unique device, and more.

    Wikimedia API License and Terms of Use: https://foundation.wikimedia.org/wiki/Policy:Terms_of_Use
    
    The Wikimedia API allows users to freely access and engage with its content. We can read and print articles or media at no cost, and are also free to share and reuse this content, provided it is done under open licenses hence making it appropriate for our usecase.

- **ORES API:** https://www.mediawiki.org/wiki/ORES

    (Objective Revision Evaluation Service) is a web service and API that provides machine learning as a service for Wikimedia projects maintained by the Machine Learning team. The system is designed to help automate critical wiki-work – for example, vandalism detection and removal. Currently, the two general types of scores that ORES generates are in the context of “edit quality” and “article quality.”

    Liftwing API Documentation: https://api.wikimedia.org/wiki/Lift_Wing_API/Reference/Get_revscoring_articlequality_prediction

- **Homework_2 template and data**: 

    - [politicians_by_country.AUG.2024.csv](https://drive.google.com/file/d/1UZ9QUYQ1R2T3Nzau85ToSNkvEzXwLUtf/view?usp=sharing):
        The file contains three columns: name (the politician's name), url (the Wikipedia URL of the politician), and country (the country to which the politician belongs)
    - [population_by_country_AUG.2024.csv](https://drive.google.com/file/d/1PlBRdx1t2eSCymXmOqtWXFJPiMn_gTQS/view?usp=sharing):
        The file contains two columns: Geography (which includes regions and countries) and Population (representing the population in millions). Regions are indicated by uppercase names (e.g., AFRICA, NORTHERN AFRICA), while individual countries are listed with their specific population values.
    - [wp_ores_liftwing_example.ipynb](https://drive.google.com/file/d/1GN1ULxKombHRzVsNKzj7tBhnBrSWUWXc/view?usp=drive_link):
        sample code to illustrate the LiftWing ORES API request for article quality
    - [wp_page_info_example.ipynb](https://drive.google.com/file/d/1iGH_pOMlspeCwDzKCPRlQdq73iS16R6k/view):
        sample code to illustrate the PageInfo API request to get the revision ID required for fetching the article quality

    Work by: Prof. David MCDonald, Human Centered Design & Engineering (HCDE), University of Washington
    <br>
    https://www.hcde.washington.edu/mcdonald
    <br>
    dwmc@uw.edu
    The content is shared with the [CC-BY](https://creativecommons.org/licenses/by/4.0/) license. This allows us to copy, resdistribute, remix or transform the material but with appropriate credit and attribution.

### Output files

- **wp_politicians_by_country.csv**

    The final output file contains detailed information about Wikipedia articles on political figures, including their respective qualities and associated demographic data. Each entry provides insights into the article's title, URL, country, quality rating, revision ID, geographic region, and population.

    #### Schema

    | Column          | Description                                                |
    |-----------------|------------------------------------------------------------|
    | article_title   | The title of the Wikipedia article                          |
    | url             | The URL link to the Wikipedia article                       |
    | country         | The country associated with the politician                  |
    | article_quality  | The quality rating of the article (e.g., Start, B, Stub)   |
    | revision_id     | The revision ID of the Wikipedia article                    |
    | region          | The geographic region of the country                        |
    | population      | The population of the country in millions                  |

<br>

- **wp_countries-no_match.txt**

    The file contains a list of countries that were identified as having no corresponding matches during the data merging process for the population and politician data

- **error_titles.txt**

    The file contains titles of articles for which we couldn't find a revision id and ORES article quality score

### Research Implications

#### Findings

The study presents some intriguing findings. Initially, my null hypothesis was that countries with larger populations and political influence would have both a higher number of total articles and higher-quality articles. However, the results reveal some contrasting patterns. Smaller countries like Antigua and Barbuda, Tonga, and Barbados stood out with the highest normalized article volume. This could be due to two main factors: the smaller populations of these countries and the absence of data from other "potentially influential" nations and regions. For instance, in the `wp_countries-no_match.txt` file, countries with more prominent political climates, such as the USA, Canada, China, the UK, and Ireland, have no articles in the politicians dataset. This missing data skews the hypothesis, causing other countries to display higher values. 

Furthermore, because the number of "good" articles is typically low per country, the population (in millions) becomes a critical factor, as seen with countries like China and India. We observe that nations with smaller populations appear to have a higher volume and quality of articles, which may be a misleading signal due to the missing data from other nations. Additionally, the data highlights that countries like Nigeria and Poland, despite their large populations and historical importance, have no high-quality articles. This discrepancy suggests potential bias in the editorial priorities of the information sources used, implying that some countries may be overlooked in favor of others, regardless of their global significance.

### Potential Biases
The findings may reflect several biases inherent in data collection and representation. Firstly, **geographic bias** could be witnessed, with a tendency to focus on specific regions, particularly Western and developed countries, while neglecting less visible or underrepresented countries. This is further compounded by **cultural bias**, where countries with rich historical affluence and greater international media presence may generate more articles. The **publication bias** also likely influences the quality and quantity of articles, as high-quality content may be more readily produced for regions that are already well-covered, thereby reinforcing existing biases. Lastly, **resources bias** might be present, where funding and research efforts are disproportionately directed toward specific regions, creating a viscious circle of underrepresentation. These biases highlight the importance of striving for a more objective and transparent approach to information distribution, ensuring that all countries receive equal contention.

### English Wikipedia as a reliable data source?
Using English Wikipedia as a data source offered clear benefits, such as well-defined article quality metrics (e.g., Stub, Start, B, Featured) that support machine learning models (ORES) and broad coverage, even of small countries like Tuvalu or Bhutan. Wikipedia's open accessibility makes it a valuable resource for data-driven research.

However, the analysis reveals significant limitations. Major countries like Canada, United States, and China are missing articles, creating sampling biases. Inconsistent labeling of countries and regions (e.g., Guinea-Bissau, North and South Korea) introduces data quality issues. The difficulty of merging datasets due to inconsistent country names and unmatched keys further complicates research. Additionally, disparities in article quality (e.g., Nigeria and India having few high-quality articles) suggest uneven editorial focus, which can skew models and further data analysis

### Can there still be usibility despite of biases and limitations?
One concrete example where Wikipedia data could be appropriate is training a content **recommendation system for educational platforms in developing countries**. Despite missing data or inconsistencies, Wikipedia offers a broad and accessible knowledge base that can serve as a foundation for identifying knowledge gaps. For instance, an organization focused on improving education in Sub-Saharan Africa could use the existing article quality and coverage data to identify countries (like Ghana or Zambia) with low-quality or sparse articles. They could then recommend areas where more content or updates are needed. While limitations exist, especially with missing articles from countries like Mauritius or Western Sahara, the model could still serve a valuable role in guiding content distribution and enrichment initiatives.
This approach would allow the organization to prioritize resources, ensuring better and fair knowledge spread where it's needed most, while acknowledging and addressing the limitations of the dataset.

### Potential Solutions to mitigate the Bias
- Integrate additional external, reliable and diverse data sources
- Improve Data labelling consistency
- Leverage crowdsourcing to fill in the missing content
- Developing clear and concrte guidelines or framworks to share and collect information
