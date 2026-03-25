# Customer Segmentation Analysis - Unsupervised Learning

## The Story Behind This Analysis

Four weeks ago, I started this journey thinking data science was about running pre-built functions. Today, I'm telling stories that data reveals—and the bridge between what I knew (SQL) and what I was learning (Python) made all the difference.

This project applies unsupervised learning techniques to segment wholesale customers based on their purchasing patterns. Using the UCI Wholesale Customers dataset, I identify distinct customer groups to inform business strategy and marketing decisions.

---

## Dataset

- **Source**: [UCI Wholesale Customers Dataset](https://archive.ics.uci.edu/dataset/292/wholesale+customers)
- **Samples**: 440 wholesale customers
- **Features**: Annual spending on 6 product categories (Fresh, Milk, Grocery, Frozen, Detergents_Paper, Delicatessen)
- **Additional attributes**: Channel (Horeca/Retail), Region (Lisbon, Oporto, Other)

---

## Analysis Pipeline

### 1. Data Preprocessing (SQL Mindset Activated)

My first instinct was to write a SQL query. Instead, I wrote this:

```python
# My SQL brain: SELECT Channel, AVG(Fresh), AVG(Milk) FROM customers GROUP BY Channel
df.groupby('Channel')[['Fresh', 'Milk', 'Grocery']].mean()
That moment, I realized pandas wasn't foreign—it was SQL with different punctuation.

Scaled features using StandardScaler (without scaling, Grocery dominated everything)

Handled categorical variables (Channel, Region)

Prepared clean dataset for dimensionality reduction and clustering

2. Dimensionality Reduction (PCA)
PCA does what a smart SQL query would do—it finds which columns explain the most variance.

The Results:

3 components retained explaining 78% of total variance

PC1: 44.2% variance explained

PC2: 22.8% variance explained

PC3: 11.0% variance explained

What PC1 Actually Told Me:

Looking at the loadings:

Detergents_Paper: +0.72

Grocery: +0.68

Milk: +0.52

Fresh: -0.45

PC1 separates household essentials buyers (positive side) from fresh food buyers (negative side). This isn't just a mathematical component—it's a business reality. You're either a grocery-focused retailer or a fresh-focused restaurant supplier. Rarely both.

PC2's Story:

Frozen: +0.65

Delicatessen: +0.58

Fresh: -0.35

PC2 separates frozen/deli specialists from fresh buyers—a secondary business distinction within the main split.

3. Clustering (KMeans)
Implemented KMeans to identify customer segments

Tested k=2 through k=10

Optimized using inertia (elbow method) and silhouette score

Visualized cluster characteristics

The Numbers:

Optimal k = 3 clusters

Silhouette score (original data): 0.35

Silhouette score (PCA-reduced): 0.38

PCA improved clustering by 8.5%—dimensionality reduction removed noise

4. Comparative Analysis
Compared clustering performance on original scaled data vs PCA-reduced data

PCA-reduced data produced clearer, more distinct segments

Key Results
Optimal Clusters
Identified 3 distinct customer segments

Selected based on silhouette score peaking at k=3 and elbow curve showing diminishing returns

PCA Components
3 components retained explaining 78% of variance

PC1: Household essentials (Detergents, Grocery, Milk) vs Fresh products—separates grocery retailers from restaurants

PC2: Frozen/Delicatessen specialists vs Fresh buyers—distinguishes specialty food retailers

Cluster Profiles
Cluster	Size	Key Characteristics	Business Insight
Cluster 0: Retail Giants	38%	High: Grocery, Detergents_Paper, Milk
Low: Fresh, Frozen	Traditional grocery stores. Focus promotions on household essentials bundles. Optimize shelf-stable inventory.
Cluster 1: Fresh Specialists	42%	Very High: Fresh
Low: All other categories	Restaurants, hotels, cafeterias (HORECA). Fresh supply chain is priority. They need reliability, not discounts on packaged goods.
Cluster 2: Specialty Retailers	20%	High: Frozen, Delicatessen
Moderate: All others	Butcher shops, delis, gourmet stores. Cross-sell opportunities between Frozen and Delicatessen. Hybrid model needing balanced inventory.
Visualizations
Scree plot: Shows cumulative variance explained by principal components

Elbow curve: Inertia decreasing across k values

Silhouette scores: Peaks at k=3 confirming optimal clusters

2D cluster visualization: Clusters plotted in PCA space showing clear separation

What I Actually Learned
Technical Lessons
PCA isn't magic—it's smart aggregation

It combines correlated features into independent components

Like creating a VIEW that simplifies complex joins, but with math

Scaling isn't optional

First run without scaling: clusters were just "who bought the most Grocery"

After StandardScaler: actual business segments emerged

Distance-based algorithms need normalized features

More clusters isn't better

Inertia kept decreasing, but silhouette score told me 3 was optimal

Lesson: Use multiple metrics, not just one

PCA improved clustering

Silhouette went from 0.35 to 0.38

Less noise = cleaner clusters

Data Science Mindset Shifts
Interpretability > Complexity

KMeans + PCA gave me clusters I can explain to a business stakeholder

Complex models don't matter if you can't act on them

Visualizations are SQL queries made visual

Scree plot: SELECT component, variance ORDER BY component

Elbow curve: SELECT k, inertia GROUP BY k

This framing helped me understand what I was actually doing

Let the data speak

I expected 4-5 clusters (one per product category)

Data said 3 clusters (business models)

I learned to stop forcing my assumptions onto the data

The SQL-to-Python Bridge: What Actually Clicked
Week 1: I was copying code. iloc vs loc confused me. I kept thinking "how do I write a WHERE clause in this?"

Week 2: I discovered df[df['Fresh'] > 5000] is literally SELECT * WHERE Fresh > 5000. The light bulb flickered.

Week 3: df.groupby('Cluster').agg({'Fresh': 'mean', 'Milk': 'sum'}) became my most-used pattern. I realized I wasn't learning new logic—I was learning new syntax for logic I already knew from SQL.

Week 4: I stopped translating. I started thinking in dataframes.

The Translation Table That Helped Me
SQL	Python (pandas)
SELECT col1, AVG(col2)	df.groupby('col1')['col2'].mean()
WHERE col1 > value	df[df['col1'] > value]
GROUP BY col1, col2	df.groupby(['col1', 'col2'])
JOIN ON key	pd.merge(df1, df2, on='key')
CASE WHEN	np.where() or df.apply()
ORDER BY col1 DESC	df.sort_values('col1', ascending=False)
The Milestone Moment
The moment I knew I'd crossed a threshold wasn't when my silhouette score hit 0.38.

It was when I looked at Cluster 1 (Fresh Specialists) and realized:

These aren't just "high Fresh buyers"

These are restaurants and hotels

They don't need detergent promotions

They need reliable fresh supply chains

In SQL, I would have stopped at SELECT * FROM customers WHERE Channel = 'Horeca'. I would have reported "Horeca customers buy more Fresh."

In Python, I kept going. I asked: "What else distinguishes them?" "How do they spend across categories?" "What business decisions does this inform?"

Python didn't just give me a different tool. It gave me a different way of thinking.

Technologies Used
Python 3.x: The language that let me think in data

pandas: My SQL brain in Python form

scikit-learn: PCA, KMeans, StandardScaler—the workhorses

matplotlib & seaborn: Making data visual (literally)

Jupyter Notebook: My lab notebook for this journey

