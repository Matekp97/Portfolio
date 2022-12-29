```python
import pandas as pd
import re
import numpy as np
pd.options.mode.chained_assignment = None
import matplotlib.pyplot as plt
import seaborn as sns
```


```python
edx_db = pd.read_csv(r"C:\Users\Mattia\Desktop\DataScience\edx_courses.csv")
edx_db
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>summary</th>
      <th>n_enrolled</th>
      <th>course_type</th>
      <th>institution</th>
      <th>instructors</th>
      <th>Level</th>
      <th>subject</th>
      <th>language</th>
      <th>subtitles</th>
      <th>course_effort</th>
      <th>course_length</th>
      <th>price</th>
      <th>course_description</th>
      <th>course_syllabus</th>
      <th>course_url</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>How to Learn Online</td>
      <td>Learn essential strategies for successful onli...</td>
      <td>124,980</td>
      <td>Self-paced on your time</td>
      <td>edX</td>
      <td>Nina Huntemann-Robyn Belair-Ben Piscopo</td>
      <td>Introductory</td>
      <td>Education &amp; Teacher Training</td>
      <td>English</td>
      <td>English</td>
      <td>2–3 hours per week</td>
      <td>2 Weeks</td>
      <td>FREE-Add a Verified Certificate for $49 USD</td>
      <td>Designed for those who are new to elearning, t...</td>
      <td>Welcome - We start with opportunities to meet ...</td>
      <td>https://www.edx.org/course/how-to-learn-online</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Programming for Everybody (Getting Started wit...</td>
      <td>This course is a "no prerequisite" introductio...</td>
      <td>293,864</td>
      <td>Self-paced on your time</td>
      <td>The University of Michigan</td>
      <td>Charles Severance</td>
      <td>Introductory</td>
      <td>Computer Science</td>
      <td>English</td>
      <td>English</td>
      <td>2–4 hours per week</td>
      <td>7 Weeks</td>
      <td>FREE-Add a Verified Certificate for $49 USD</td>
      <td>This course aims to teach everyone the basics ...</td>
      <td>NaN</td>
      <td>https://www.edx.org/course/programming-for-eve...</td>
    </tr>
    <tr>
      <td>2</td>
      <td>CS50's Introduction to Computer Science</td>
      <td>An introduction to the intellectual enterprise...</td>
      <td>2,442,271</td>
      <td>Self-paced on your time</td>
      <td>Harvard University</td>
      <td>David J. Malan-Doug Lloyd-Brian Yu</td>
      <td>Introductory</td>
      <td>Computer Science</td>
      <td>English</td>
      <td>English</td>
      <td>6–18 hours per week</td>
      <td>12 Weeks</td>
      <td>FREE-Add a Verified Certificate for $90 USD</td>
      <td>This is CS50x , Harvard University's introduct...</td>
      <td>NaN</td>
      <td>https://www.edx.org/course/cs50s-introduction-...</td>
    </tr>
    <tr>
      <td>3</td>
      <td>The Analytics Edge</td>
      <td>Through inspiring examples and stories, discov...</td>
      <td>129,555</td>
      <td>Instructor-led on a course schedule</td>
      <td>Massachusetts Institute of Technology</td>
      <td>Dimitris Bertsimas-Allison O'Hair-John Silberh...</td>
      <td>Intermediate</td>
      <td>Data Analysis &amp; Statistics</td>
      <td>English</td>
      <td>English</td>
      <td>10–15 hours per week</td>
      <td>13 Weeks</td>
      <td>FREE-Add a Verified Certificate for $199 USD</td>
      <td>In the last decade, the amount of data availab...</td>
      <td>NaN</td>
      <td>https://www.edx.org/course/the-analytics-edge</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Marketing Analytics: Marketing Measurement Str...</td>
      <td>This course is part of a MicroMasters® Program</td>
      <td>81,140</td>
      <td>Self-paced on your time</td>
      <td>University of California, Berkeley</td>
      <td>Stephan Sorger</td>
      <td>Introductory</td>
      <td>Computer Science</td>
      <td>English</td>
      <td>English</td>
      <td>5–7 hours per week</td>
      <td>4 Weeks</td>
      <td>FREE-Add a Verified Certificate for $249 USD</td>
      <td>Begin your journey in a new career in marketin...</td>
      <td>NaN</td>
      <td>https://www.edx.org/course/marketing-analytics...</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>970</td>
      <td>Leaders in Citizen Security and Justice Manage...</td>
      <td>Learn about the latest in prevention, police a...</td>
      <td>NaN</td>
      <td>Self-paced on your time</td>
      <td>Inter-American Development Bank</td>
      <td>Olga Espinoza-Eduardo Pazinato-Alejandra Mera-...</td>
      <td>Intermediate</td>
      <td>Social Sciences</td>
      <td>English</td>
      <td>English</td>
      <td>4–5 hours per week</td>
      <td>10 Weeks</td>
      <td>FREE-Add a Verified Certificate for $25 USD</td>
      <td>The high rates of crime and violence are two o...</td>
      <td>NaN</td>
      <td>https://www.edx.org/course/leaders-in-citizen-...</td>
    </tr>
    <tr>
      <td>971</td>
      <td>Pattern Studying and Making | 图案审美与创作</td>
      <td>Fantastic experiences in beauty and its repres...</td>
      <td>NaN</td>
      <td>Self-paced on your time</td>
      <td>Tsinghua University</td>
      <td>Yuehua Nie</td>
      <td>Introductory</td>
      <td>Art &amp; Culture</td>
      <td>中文</td>
      <td>English, 中文</td>
      <td>3–5 hours per week</td>
      <td>12 Weeks</td>
      <td>FREE-Add a Verified Certificate for $139 USD</td>
      <td>Are you an original designer? Or a DIY fancier...</td>
      <td>NaN</td>
      <td>https://www.edx.org/course/pattern-studying-an...</td>
    </tr>
    <tr>
      <td>972</td>
      <td>Computational Neuroscience: Neuronal Dynamics ...</td>
      <td>This course explains the mathematical and comp...</td>
      <td>11,246</td>
      <td>Self-paced on your time</td>
      <td>École polytechnique fédérale de Lausanne</td>
      <td>Wulfram Gerstner</td>
      <td>Advanced</td>
      <td>Biology &amp; Life Sciences</td>
      <td>English</td>
      <td>English</td>
      <td>4–6 hours per week</td>
      <td>6 Weeks</td>
      <td>FREE-Add a Verified Certificate for $139 USD</td>
      <td>What happens in your brain when you make a dec...</td>
      <td>Textbook: Neuronal Dynamics - from single neur...</td>
      <td>https://www.edx.org/course/computational-neuro...</td>
    </tr>
    <tr>
      <td>973</td>
      <td>Cities and the Challenge of Sustainable Develo...</td>
      <td>What is a sustainable city? Learn the basics h...</td>
      <td>8,775</td>
      <td>Self-paced on your time</td>
      <td>SDG Academy</td>
      <td>Jeffrey D. Sachs</td>
      <td>Introductory</td>
      <td>Environmental Studies</td>
      <td>English</td>
      <td>English</td>
      <td>1–2 hours per week</td>
      <td>1 Weeks</td>
      <td>FREE-Add a Verified Certificate for $25 USD</td>
      <td>According to the United Nations, urbanization ...</td>
      <td>Module 1: Introduction to the SDGsProfessor Je...</td>
      <td>https://www.edx.org/course/cities-and-the-chal...</td>
    </tr>
    <tr>
      <td>974</td>
      <td>MathTrackX: Special Functions</td>
      <td>Understand trigonometric, exponential and loga...</td>
      <td>NaN</td>
      <td>Self-paced on your time</td>
      <td>University of Adelaide</td>
      <td>Dr David Butler</td>
      <td>Introductory</td>
      <td>Math</td>
      <td>English</td>
      <td>English</td>
      <td>3–6 hours per week</td>
      <td>4 Weeks</td>
      <td>FREE-Add a Verified Certificate for $79 USD</td>
      <td>This course is part two of the MathTrackX XSer...</td>
      <td>NaN</td>
      <td>https://www.edx.org/course/mathtrackx-special-...</td>
    </tr>
  </tbody>
</table>
<p>975 rows × 16 columns</p>
</div>




```python
edx_db2 = edx_db[["title", "price", "course_length", "n_enrolled", "institution","Level","subject","course_type","course_effort"]]
edx_db2["price"]=edx_db2["price"].str.extract('(\d{1,}(.\d{1,})?)')
edx_db2["course_effort"] = edx_db2["course_effort"].str.replace(' hours per week', '')
edx_db2["course_length"] = edx_db2["course_length"].str.replace('Weeks', '')
edx_db2['course_length'] = edx_db2['course_length'].astype(int)
edx_db2["media_effort"] = edx_db2.apply(lambda x: x["course_length"]*np.mean(list(map(int, x["course_effort"].split("–")))), axis=1)
edx_db2["course_length"] = edx_db2.apply(lambda x: x["course_length"]*np.mean(list(map(int, x["course_effort"].split("–")))), axis=1)
```


```python
edx_db2 = edx_db[["title", "price", "course_length", "n_enrolled", "institution","Level","subject","course_type","course_effort"]]
edx_db2["price"]=edx_db2["price"].str.extract('(\d{1,}(.\d{1,})?)')
edx_db2["course_effort"] = edx_db2["course_effort"].str.replace(' hours per week', '')
edx_db2["course_length"] = edx_db2["course_length"].str.replace('Weeks', '')
edx_db2['course_length'] = edx_db2['course_length'].astype(int)
edx_db2["course_length"] = edx_db2.apply(lambda x: x["course_length"]*np.mean(list(map(int, x["course_effort"].split("–")))), axis=1)
edx_db2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>price</th>
      <th>course_length</th>
      <th>n_enrolled</th>
      <th>institution</th>
      <th>Level</th>
      <th>subject</th>
      <th>course_type</th>
      <th>course_effort</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>How to Learn Online</td>
      <td>49</td>
      <td>5.0</td>
      <td>124,980</td>
      <td>edX</td>
      <td>Introductory</td>
      <td>Education &amp; Teacher Training</td>
      <td>Self-paced on your time</td>
      <td>2–3</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Programming for Everybody (Getting Started wit...</td>
      <td>49</td>
      <td>21.0</td>
      <td>293,864</td>
      <td>The University of Michigan</td>
      <td>Introductory</td>
      <td>Computer Science</td>
      <td>Self-paced on your time</td>
      <td>2–4</td>
    </tr>
    <tr>
      <td>2</td>
      <td>CS50's Introduction to Computer Science</td>
      <td>90</td>
      <td>144.0</td>
      <td>2,442,271</td>
      <td>Harvard University</td>
      <td>Introductory</td>
      <td>Computer Science</td>
      <td>Self-paced on your time</td>
      <td>6–18</td>
    </tr>
    <tr>
      <td>3</td>
      <td>The Analytics Edge</td>
      <td>199</td>
      <td>162.5</td>
      <td>129,555</td>
      <td>Massachusetts Institute of Technology</td>
      <td>Intermediate</td>
      <td>Data Analysis &amp; Statistics</td>
      <td>Instructor-led on a course schedule</td>
      <td>10–15</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Marketing Analytics: Marketing Measurement Str...</td>
      <td>249</td>
      <td>24.0</td>
      <td>81,140</td>
      <td>University of California, Berkeley</td>
      <td>Introductory</td>
      <td>Computer Science</td>
      <td>Self-paced on your time</td>
      <td>5–7</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>970</td>
      <td>Leaders in Citizen Security and Justice Manage...</td>
      <td>25</td>
      <td>45.0</td>
      <td>NaN</td>
      <td>Inter-American Development Bank</td>
      <td>Intermediate</td>
      <td>Social Sciences</td>
      <td>Self-paced on your time</td>
      <td>4–5</td>
    </tr>
    <tr>
      <td>971</td>
      <td>Pattern Studying and Making | 图案审美与创作</td>
      <td>139</td>
      <td>48.0</td>
      <td>NaN</td>
      <td>Tsinghua University</td>
      <td>Introductory</td>
      <td>Art &amp; Culture</td>
      <td>Self-paced on your time</td>
      <td>3–5</td>
    </tr>
    <tr>
      <td>972</td>
      <td>Computational Neuroscience: Neuronal Dynamics ...</td>
      <td>139</td>
      <td>30.0</td>
      <td>11,246</td>
      <td>École polytechnique fédérale de Lausanne</td>
      <td>Advanced</td>
      <td>Biology &amp; Life Sciences</td>
      <td>Self-paced on your time</td>
      <td>4–6</td>
    </tr>
    <tr>
      <td>973</td>
      <td>Cities and the Challenge of Sustainable Develo...</td>
      <td>25</td>
      <td>1.5</td>
      <td>8,775</td>
      <td>SDG Academy</td>
      <td>Introductory</td>
      <td>Environmental Studies</td>
      <td>Self-paced on your time</td>
      <td>1–2</td>
    </tr>
    <tr>
      <td>974</td>
      <td>MathTrackX: Special Functions</td>
      <td>79</td>
      <td>18.0</td>
      <td>NaN</td>
      <td>University of Adelaide</td>
      <td>Introductory</td>
      <td>Math</td>
      <td>Self-paced on your time</td>
      <td>3–6</td>
    </tr>
  </tbody>
</table>
<p>975 rows × 9 columns</p>
</div>




```python
edx_db2['n_enrolled'] = edx_db2['n_enrolled'].str.replace(",","")
edx_db2['n_enrolled'] = edx_db2['n_enrolled'].fillna(0).astype(int)
edx_db2['price'] = edx_db2['price'].astype(float)
edx_db2['Level'] = edx_db2['Level'].astype('category')
edx_db2['Level'] = edx_db2['Level'].cat.codes
edx_db2['course_type'] = edx_db2['course_type'].astype('category')
edx_db2['course_type'] = edx_db2['course_type'].cat.codes
```


```python
edx_db2 = edx_db2.sort_values(by=['price','title'])
sns.barplot(x = 'price', y = 'title', data = edx_db2[(edx_db2.price>200)&(edx_db2.subject == 'Computer Science')], color = 'blue', edgecolor='black')
```




    <AxesSubplot:xlabel='price', ylabel='title'>




    
![png](output_5_1.png)
    



```python
sns.barplot(x = 'n_enrolled', y = 'title', data = edx_db2[(edx_db2.price>200)&(edx_db2.subject == 'Computer Science')], color = 'blue', edgecolor='black')
```




    <AxesSubplot:xlabel='n_enrolled', ylabel='title'>




    
![png](output_6_1.png)
    



```python
sns.scatterplot(x = 'course_length', y = 'price', data = edx_db2)
```




    <AxesSubplot:xlabel='course_length', ylabel='price'>




    
![png](output_7_1.png)
    



```python
edx_db2[["price","n_enrolled","course_length"]].mean()
```




    price              100.465497
    n_enrolled       46705.230769
    course_length       34.405641
    dtype: float64




```python
edx_db_user = pd.read_csv(r"C:\Users\Mattia\Desktop\DataScience\appendix.csv")
edx_db_user
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Institution</th>
      <th>Course Number</th>
      <th>Launch Date</th>
      <th>Course Title</th>
      <th>Instructors</th>
      <th>Course Subject</th>
      <th>Year</th>
      <th>Honor Code Certificates</th>
      <th>Participants (Course Content Accessed)</th>
      <th>Audited (&gt; 50% Course Content Accessed)</th>
      <th>...</th>
      <th>% Certified of &gt; 50% Course Content Accessed</th>
      <th>% Played Video</th>
      <th>% Posted in Forum</th>
      <th>% Grade Higher Than Zero</th>
      <th>Total Course Hours (Thousands)</th>
      <th>Median Hours for Certification</th>
      <th>Median Age</th>
      <th>% Male</th>
      <th>% Female</th>
      <th>% Bachelor's Degree or Higher</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>MITx</td>
      <td>6.002x</td>
      <td>09/05/2012</td>
      <td>Circuits and Electronics</td>
      <td>Khurram Afridi</td>
      <td>Science, Technology, Engineering, and Mathematics</td>
      <td>1</td>
      <td>1</td>
      <td>36105</td>
      <td>5431</td>
      <td>...</td>
      <td>54.98</td>
      <td>83.2</td>
      <td>8.17</td>
      <td>28.97</td>
      <td>418.94</td>
      <td>64.45</td>
      <td>26.0</td>
      <td>88.28</td>
      <td>11.72</td>
      <td>60.68</td>
    </tr>
    <tr>
      <td>1</td>
      <td>MITx</td>
      <td>6.00x</td>
      <td>09/26/2012</td>
      <td>Introduction to Computer Science and Programming</td>
      <td>Eric Grimson, John Guttag, Chris Terman</td>
      <td>Computer Science</td>
      <td>1</td>
      <td>1</td>
      <td>62709</td>
      <td>8949</td>
      <td>...</td>
      <td>64.05</td>
      <td>89.14</td>
      <td>14.38</td>
      <td>39.50</td>
      <td>884.04</td>
      <td>78.53</td>
      <td>28.0</td>
      <td>83.50</td>
      <td>16.50</td>
      <td>63.04</td>
    </tr>
    <tr>
      <td>2</td>
      <td>MITx</td>
      <td>3.091x</td>
      <td>10/09/2012</td>
      <td>Introduction to Solid State Chemistry</td>
      <td>Michael Cima</td>
      <td>Science, Technology, Engineering, and Mathematics</td>
      <td>1</td>
      <td>1</td>
      <td>16663</td>
      <td>2855</td>
      <td>...</td>
      <td>72.85</td>
      <td>87.49</td>
      <td>14.42</td>
      <td>34.89</td>
      <td>227.55</td>
      <td>61.28</td>
      <td>27.0</td>
      <td>70.32</td>
      <td>29.68</td>
      <td>58.76</td>
    </tr>
    <tr>
      <td>3</td>
      <td>HarvardX</td>
      <td>CS50x</td>
      <td>10/15/2012</td>
      <td>Introduction to Computer Science</td>
      <td>David Malan, Nate Hardison, Rob Bowden, Tommy ...</td>
      <td>Computer Science</td>
      <td>1</td>
      <td>1</td>
      <td>129400</td>
      <td>12888</td>
      <td>...</td>
      <td>11.11</td>
      <td>0</td>
      <td>0.00</td>
      <td>1.11</td>
      <td>220.90</td>
      <td>0.00</td>
      <td>28.0</td>
      <td>80.02</td>
      <td>19.98</td>
      <td>58.78</td>
    </tr>
    <tr>
      <td>4</td>
      <td>HarvardX</td>
      <td>PH207x</td>
      <td>10/15/2012</td>
      <td>Health in Numbers: Quantitative Methods in Cli...</td>
      <td>Earl Francis Cook, Marcello Pagano</td>
      <td>Government, Health, and Social Science</td>
      <td>1</td>
      <td>1</td>
      <td>52521</td>
      <td>10729</td>
      <td>...</td>
      <td>47.12</td>
      <td>77.45</td>
      <td>15.98</td>
      <td>32.52</td>
      <td>804.41</td>
      <td>76.10</td>
      <td>32.0</td>
      <td>56.78</td>
      <td>43.22</td>
      <td>88.33</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>285</td>
      <td>HarvardX</td>
      <td>MUS24.4x</td>
      <td>07/21/2016</td>
      <td>First Nights: Symphonie Fantastique</td>
      <td>Tom Kelly</td>
      <td>Humanities, History, Design, Religion, and Edu...</td>
      <td>4</td>
      <td>0</td>
      <td>615</td>
      <td>305</td>
      <td>...</td>
      <td>6.56</td>
      <td>80.81</td>
      <td>8.78</td>
      <td>3.25</td>
      <td>1.71</td>
      <td>5.93</td>
      <td>38.0</td>
      <td>56.82</td>
      <td>43.18</td>
      <td>74.66</td>
    </tr>
    <tr>
      <td>286</td>
      <td>HarvardX</td>
      <td>GSE4x</td>
      <td>07/25/2016</td>
      <td>Introduction to Family Engagement in Education</td>
      <td>Karen Mapp</td>
      <td>Humanities, History, Design, Religion, and Edu...</td>
      <td>4</td>
      <td>0</td>
      <td>2871</td>
      <td>267</td>
      <td>...</td>
      <td>7.49</td>
      <td>70.11</td>
      <td>0.00</td>
      <td>0.70</td>
      <td>4.26</td>
      <td>11.33</td>
      <td>34.0</td>
      <td>25.24</td>
      <td>74.76</td>
      <td>82.31</td>
    </tr>
    <tr>
      <td>287</td>
      <td>MITx</td>
      <td>6.302.0x</td>
      <td>08/01/2016</td>
      <td>Introduction to Control System Design</td>
      <td>Jacob White, Joe Steinmeyer</td>
      <td>Science, Technology, Engineering, and Mathematics</td>
      <td>4</td>
      <td>0</td>
      <td>3937</td>
      <td>974</td>
      <td>...</td>
      <td>5.03</td>
      <td>12.27</td>
      <td>4.72</td>
      <td>8.23</td>
      <td>15.62</td>
      <td>58.50</td>
      <td>24.0</td>
      <td>91.17</td>
      <td>8.83</td>
      <td>61.32</td>
    </tr>
    <tr>
      <td>288</td>
      <td>MITx</td>
      <td>6.302.1x</td>
      <td>08/01/2016</td>
      <td>Introduction to State Space Control</td>
      <td>Jacob White, Joe Steinmeyer</td>
      <td>Science, Technology, Engineering, and Mathematics</td>
      <td>4</td>
      <td>0</td>
      <td>1431</td>
      <td>208</td>
      <td>...</td>
      <td>3.85</td>
      <td>0</td>
      <td>3.84</td>
      <td>5.73</td>
      <td>3.22</td>
      <td>62.38</td>
      <td>25.0</td>
      <td>93.44</td>
      <td>6.56</td>
      <td>72.31</td>
    </tr>
    <tr>
      <td>289</td>
      <td>MITx</td>
      <td>3.15.3x</td>
      <td>08/03/2016</td>
      <td>Magnetic Materials and Devices</td>
      <td>Caroline Ross</td>
      <td>Science, Technology, Engineering, and Mathematics</td>
      <td>4</td>
      <td>0</td>
      <td>1294</td>
      <td>364</td>
      <td>...</td>
      <td>10.44</td>
      <td>49.92</td>
      <td>4.40</td>
      <td>14.30</td>
      <td>6.87</td>
      <td>40.59</td>
      <td>25.0</td>
      <td>85.95</td>
      <td>14.05</td>
      <td>65.78</td>
    </tr>
  </tbody>
</table>
<p>290 rows × 23 columns</p>
</div>




```python
for i in edx_db_user["Course Subject"].unique():
    print(str(i) + ": " + str(len(edx_db_user[edx_db_user["Course Subject"]==i])))
```

    Science, Technology, Engineering, and Mathematics: 91
    Computer Science: 30
    Government, Health, and Social Science: 75
    Humanities, History, Design, Religion, and Education: 94
    


```python
edx_db_user[["Course Subject","Participants (Course Content Accessed)","Certified", "Median Hours for Certification", "Median Age"]].groupby(["Course Subject"], sort=False).sum()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Participants (Course Content Accessed)</th>
      <th>Certified</th>
      <th>Median Hours for Certification</th>
      <th>Median Age</th>
    </tr>
    <tr>
      <th>Course Subject</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Science, Technology, Engineering, and Mathematics</td>
      <td>1082060</td>
      <td>44878</td>
      <td>7151.01</td>
      <td>2372.5</td>
    </tr>
    <tr>
      <td>Computer Science</td>
      <td>1527334</td>
      <td>51343</td>
      <td>1349.27</td>
      <td>805.0</td>
    </tr>
    <tr>
      <td>Government, Health, and Social Science</td>
      <td>1017960</td>
      <td>82267</td>
      <td>2706.93</td>
      <td>2230.0</td>
    </tr>
    <tr>
      <td>Humanities, History, Design, Religion, and Education</td>
      <td>822503</td>
      <td>66217</td>
      <td>1658.51</td>
      <td>3089.5</td>
    </tr>
  </tbody>
</table>
</div>




```python
edx_udemy = pd.read_csv(r"C:\Users\Mattia\Desktop\DataScience\multiTimeline.csv")
```


```python
edx_udemy['Time'] = pd.to_datetime(edx_udemy['Mese'])
edx_udemy['edx'] = edx_udemy['edx: (Tutto il mondo)'].astype(float)
edx_udemy['udemy'] = edx_udemy['udemy: (Tutto il mondo)'].astype(float)
edx_udemy
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Mese</th>
      <th>edx: (Tutto il mondo)</th>
      <th>udemy: (Tutto il mondo)</th>
      <th>coursera: (Tutto il mondo)</th>
      <th>Time</th>
      <th>edx</th>
      <th>udemy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>2013-01</td>
      <td>8</td>
      <td>2</td>
      <td>29</td>
      <td>2013-01-01</td>
      <td>8.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>2013-02</td>
      <td>9</td>
      <td>2</td>
      <td>33</td>
      <td>2013-02-01</td>
      <td>9.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>2013-03</td>
      <td>9</td>
      <td>2</td>
      <td>31</td>
      <td>2013-03-01</td>
      <td>9.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>2013-04</td>
      <td>9</td>
      <td>2</td>
      <td>29</td>
      <td>2013-04-01</td>
      <td>9.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>2013-05</td>
      <td>8</td>
      <td>2</td>
      <td>32</td>
      <td>2013-05-01</td>
      <td>8.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>115</td>
      <td>2022-08</td>
      <td>11</td>
      <td>98</td>
      <td>49</td>
      <td>2022-08-01</td>
      <td>11.0</td>
      <td>98.0</td>
    </tr>
    <tr>
      <td>116</td>
      <td>2022-09</td>
      <td>11</td>
      <td>90</td>
      <td>45</td>
      <td>2022-09-01</td>
      <td>11.0</td>
      <td>90.0</td>
    </tr>
    <tr>
      <td>117</td>
      <td>2022-10</td>
      <td>10</td>
      <td>82</td>
      <td>45</td>
      <td>2022-10-01</td>
      <td>10.0</td>
      <td>82.0</td>
    </tr>
    <tr>
      <td>118</td>
      <td>2022-11</td>
      <td>9</td>
      <td>90</td>
      <td>44</td>
      <td>2022-11-01</td>
      <td>9.0</td>
      <td>90.0</td>
    </tr>
    <tr>
      <td>119</td>
      <td>2022-12</td>
      <td>9</td>
      <td>82</td>
      <td>41</td>
      <td>2022-12-01</td>
      <td>9.0</td>
      <td>82.0</td>
    </tr>
  </tbody>
</table>
<p>120 rows × 7 columns</p>
</div>




```python
plt.plot(edx_udemy['Time'], edx_udemy['edx'])
plt.plot(edx_udemy['Time'], edx_udemy['udemy'])
```




    [<matplotlib.lines.Line2D at 0x263d51883c8>]




    
![png](output_14_1.png)
    

