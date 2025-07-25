# The Analysis

## 1. what are the most demanded skills for the top 3 most popular data roles in Indonesia?

To find the most demanded skills for the top 3 most popular data roles in Indonesia. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills by analyzing how frequently each title appears in job postings and how often each skill is mentioned for that position.

view my notebook with detailed steps here: [skill_demand.ipynb](2_skill_demand.ipynb)

### Visualize Data
```python
df_skills_perc = pd.merge(df_skills_count, df_job_title_count, how='left', on='job_title_short' )

df_skills_perc['skill_percent'] = (df_skills_perc['skill_count'] / df_skills_perc['jobs_total']) * 100

ig, ax = plt.subplots(len(job_titles), 1)
sns.set_theme(style='ticks')

for idx, title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == title].head(5)
    sns.barplot(data=df_plot, y='job_skills', x='skill_percent', ax=ax[idx], hue='skill_count', palette='dark:b_r')
    ax[idx].set_ylabel('')
    ax[idx].set_xlabel('')
    ax[idx].legend().set_visible(False)
    ax[idx].set_title(title)
    ax[idx].set_xlim(0,70)

    for i, v in enumerate(df_plot['skill_percent']):
        ax[idx].text(v + 1, i, f'{v:.0f}%', va='center')
    
    if idx != len(job_titles) - 1:
        ax[idx].set_xticks([])

fig.suptitle('Counts of Top Skills in Job Postings')
fig.tight_layout()
plt.show()
```


### Results
![Visualization of Top Skills for Data Role in Indonesia](images\skill_demands.png)

### Insights
- SQL is the most sought after skill for data roles in Indonesia, with a 49% demand in Data Analyst positions and 57% demand in Data Engineering roles.
- Python is a versatile skill with high demand across all of the top 3 data roles in Indonesia, even surpassing SQL in demand for Data Scientist positions.
- Data Engineers require broader cloud and big data skills. Skills like Spark (23%), AWS (22%), and Hadoop(20%) show that Data Engineer roles emphasize distributed computing and cloud infrastructure.

## 2. How are in_demand skills trending for Data Engineer in Indonesia?

### Visualize Data

```python
df_plot = df_percent[top_5_skills]
sns.set_theme(style='ticks')
sns.lineplot(data=df_plot, dashes=False, palette='tab10')
sns.despine()

plt.xlabel('2025')
plt.ylabel('skills trend in Job Postings (%)')
plt.legend().remove()

sorted_dec = df_plot.iloc[-1].sort_values(ascending=False)
minimum_dist = 2.5
prev_val = -99
texts = []
for idx in range(5):
    cur_val = sorted_dec.iloc[idx] 
    if not prev_val == -99:
        diff = prev_val - cur_val
        cur_val = cur_val - (minimum_dist - diff )if diff < minimum_dist else cur_val
    texts.append(plt.text(11,cur_val , df_plot.columns[idx]))
    prev_val = cur_val 
adjust_text(texts, arrowstyle='->', color='gray', lw=0.5)

ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()
```

### Results
![Trending Top Skills for Data Engineer in Indonesia](images\skill_trending_permonth.png)

### Insights
- SQL remained the most consistently in demand skill for Data Engineer throughout the year, although it shows a gradual decline in demand
- Python showed a steady increase in demand throughout the year. Although there was a dim in September, demand surged significantly by year end. It even has the potential to overtake SQL as the most in demand skill for Data Engineer in the future
- Hadoop showed an overal decline, stabilizing at a lower demand level aftear peaking in February. This may reflect a shift away from traditional big data framework toward more modern cloud based or real time platforms. 