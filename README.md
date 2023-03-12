---
jupyter:
  colab:
    collapsed_sections:
    - fGZ_mk26LpEw
  kernelspec:
    display_name: Python 3
    name: python3
  language_info:
    name: python
  nbformat: 4
  nbformat_minor: 0
---

<div class="cell markdown" id="lQnX_kTnPlHd">

#CUHK-Stat1013 Project - Daily Mean Temperature of Tai Mo Shan & Ngong
Ping

</div>

<div class="cell markdown" id="fGZ_mk26LpEw">

##Student

</div>

<div class="cell markdown" id="Hnw76dQPLZEu">

Student Name: Chung Hau Yin

Student ID:1155193412

</div>

<div class="cell markdown" id="G5WhcXHlQ7GU">

##Background

### **Description:**

Two datasets describing the daily mean temperature in Ngong Ping and on
Tai Mo Shan .

### **downloaded from:**

<https://data.gov.hk/en-data/dataset/hk-hko-rss-daily-temperature-info-hko/resource/3e549666-c08b-40cf-8fba-d9318332ddd0>

<https://data.gov.hk/en-data/dataset/hk-hko-rss-daily-temperature-info-hko/resource/b9f4755e-dfad-4d54-9308-dccba98f7601>

note that the datasets used only cover the data up to 31th January,
2023.

###**sample size:** 800 **Feature documentation:**

####before merging (for both raw datasets) \|Feature\|Dtype\|
\|:------\|:----\| \|年/Year\|int\| \|月/Month\|int\| \|日/Day\|int\|
\|數值/Value\|float or object(for lost data)\| \|數據完整性/data
Completeness\|object\|

####after merging (df) \|Feature\|Dtype\| \|:------\|:----\|
\|年/Year\|int\| \|月/Month\|int\| \|日/Day\|int\| \|place\|object\|
\|daily mean temperature\|float\|

####after merging (df_c) \|Feature\|Dtype\| \|:------\|:----\|
\|年/Year\|int\| \|月/Month\|int\| \|日/Day\|int\| \|daily mean
temperature of Ngong Ping\|float\| \|daily mean temperature of Tai Mo
Shan\|float\|

</div>

<div class="cell markdown" id="zVrcIlLcQ7FP">

##Hypothesis

-   Tell us what your idea is and why you have chosen to pursue this
    idea.
    -   I am interested in **'In Hong Kong, is the temperature recorded
        on Tai Mo Shan lower than the temperature recorded in Ngong Ping
        in winter.'**
    -   Although they are both mountain, I want to learn about whether
        the the taller one would be colder.
-   What two groups you are comparing:
-   **G1:** daily mean temperature in Ngong Ping
-   **G2:** daily mean temperature in Tai Mo Shan
-   What you will be measuring (i.e., what your response variable will
    be)
-   'daily mean temperature'
-   Is your response variable quantitative rather than categorical?
-   'daily mean temperature' is variable quantitive.
-   Make a prediction about what kind of difference you expect to see
    between your samples and WHY:
-   I expect that **G1>G2** as Tai Mo Shan is the tallest mountain in
    Hong Kong, I expect that mean temperature recorded on Tai Mo Shan
    would be lower.
-   Talk about how you will gather your data
-   I will gather the data from <https://data.gov.hk/>
-   After I find two datasets, I will merge them with codes.While only
    selecting data with '年/Year' is between 2014 and 2023 and
    '月/Month' is between December and february.Then cancel out the lost
    data.
-   If you had unlimited resources (time, money, staff, etc.) how would
    you collect your data?
-   I would tried not only find the mean temperture of both places, but
    also the minimun and the maximum temperature of both places.

</div>

<div class="cell code" execution_count="1"
colab="{&quot;base_uri&quot;:&quot;https://localhost:8080/&quot;}"
id="QeLTdh9zJe79" outputId="81d86dcb-3c1d-4ad3-b126-48aebf343a9a">

``` python
! gdown --id 1f4vlW_ChTqcBlTUzhVTpWoo8uuuhbzia
! gdown --id 1HouwoINgcDUOdgYegoF47AStyIGj_35M
```

<div class="output stream stdout">

    /usr/local/lib/python3.8/dist-packages/gdown/cli.py:127: FutureWarning: Option `--id` was deprecated in version 4.3.1 and will be removed in 5.0. You don't need to pass it anymore to use a file ID.
      warnings.warn(
    Downloading...
    From: https://drive.google.com/uc?id=1f4vlW_ChTqcBlTUzhVTpWoo8uuuhbzia
    To: /content/CLMTEMP_NGP_.xlsx
    100% 187k/187k [00:00<00:00, 59.7MB/s]
    /usr/local/lib/python3.8/dist-packages/gdown/cli.py:127: FutureWarning: Option `--id` was deprecated in version 4.3.1 and will be removed in 5.0. You don't need to pass it anymore to use a file ID.
      warnings.warn(
    Downloading...
    From: https://drive.google.com/uc?id=1HouwoINgcDUOdgYegoF47AStyIGj_35M
    To: /content/CLMTEMP_TMS_.xlsx
    100% 232k/232k [00:00<00:00, 80.5MB/s]

</div>

</div>

<div class="cell markdown" id="Fx_-2-AzcTAf">

#Merging Dataset (df and df_c)

</div>

<div class="cell code" execution_count="2"
colab="{&quot;base_uri&quot;:&quot;https://localhost:8080/&quot;}"
id="pUmBNGGVO3DR" outputId="9f048f74-6726-4764-dabf-c1531e5a9757">

``` python
import pandas as pd
df_b = pd.read_excel('/content/CLMTEMP_TMS_.xlsx')
df_a = pd.read_excel('/content/CLMTEMP_NGP_.xlsx')


df_a['年/Year'] = pd.to_numeric(df_a['年/Year'], errors='coerce')
df_a['月/Month'] = pd.to_numeric(df_a['月/Month'], errors='coerce')
df_a['數值/Value'] = pd.to_numeric(df_a['數值/Value'], errors='coerce')
df_a= df_a.drop(columns= ['數據完整性/data Completeness'])
df_a= df_a.rename(columns={'數值/Value':'Ngong Ping'})
new_df_a = df_a[(df_a['年/Year'] >= 2014) & (df_a['年/Year'] <= 2023) & ((df_a['月/Month'] >= 12) | (df_a['月/Month'] <= 2))]
df_b['月/Month'] = pd.to_numeric(df_b['月/Month'], errors='coerce')
df_b['年/Year'] = pd.to_numeric(df_b['年/Year'], errors='coerce')
df_b['數值/Value'] = pd.to_numeric(df_b['數值/Value'], errors='coerce')
df_b= df_b.drop(columns= ['數據完整性/data Completeness'])
df_b= df_b.rename(columns={'數值/Value':'Tai Mo Shan'})
new_df_b = df_b[(df_b['年/Year'] >= 2014) & (df_b['年/Year'] <= 2023) & ((df_b['月/Month'] >= 12) | (df_b['月/Month'] <= 2))]
df_c = pd.merge(new_df_a, new_df_b, on=['年/Year', '月/Month', '日/Day'])
df_c['年/Year']=df_c['年/Year'].astype(int)
df_c['月/Month']=df_c['月/Month'].astype(int)
df_c['日/Day']=df_c['日/Day'].astype(int)
df_c = df_c.dropna()
df = pd.melt(df_c, id_vars=['年/Year', '月/Month','日/Day'], value_vars=['Ngong Ping', 'Tai Mo Shan'], var_name='place', value_name='daily mean temperature')
df_c= df_c.rename(columns={'Ngong Ping':'daily mean temperature of Ngong Ping'})
df_c= df_c.rename(columns={'Tai Mo Shan':'daily mean temperature of Tai Mo Shan'})
print('df')
print(df)
print()
print('------------------------------------------------------------------------------------------------')
print('df_c')
print(df_c)
```

<div class="output stream stdout">

    df
          年/Year  月/Month  日/Day        place  daily mean temperature
    0       2014        1      1   Ngong Ping                    14.1
    1       2014        1      2   Ngong Ping                    15.7
    2       2014        1      3   Ngong Ping                    17.5
    3       2014        1      4   Ngong Ping                    14.8
    4       2014        1      5   Ngong Ping                    13.3
    ...      ...      ...    ...          ...                     ...
    1595    2023        1     27  Tai Mo Shan                     5.9
    1596    2023        1     28  Tai Mo Shan                     3.7
    1597    2023        1     29  Tai Mo Shan                     5.4
    1598    2023        1     30  Tai Mo Shan                     9.8
    1599    2023        1     31  Tai Mo Shan                    11.4

    [1600 rows x 5 columns]

    ------------------------------------------------------------------------------------------------
    df_c
         年/Year  月/Month  日/Day  daily mean temperature of Ngong Ping  \
    0      2014        1      1                                  14.1   
    1      2014        1      2                                  15.7   
    2      2014        1      3                                  17.5   
    3      2014        1      4                                  14.8   
    4      2014        1      5                                  13.3   
    ..      ...      ...    ...                                   ...   
    838    2023        1     27                                   9.3   
    839    2023        1     28                                   6.9   
    840    2023        1     29                                   7.6   
    841    2023        1     30                                  11.4   
    842    2023        1     31                                  14.3   

         daily mean temperature of Tai Mo Shan  
    0                                     11.3  
    1                                     12.8  
    2                                     14.0  
    3                                     11.8  
    4                                      9.1  
    ..                                     ...  
    838                                    5.9  
    839                                    3.7  
    840                                    5.4  
    841                                    9.8  
    842                                   11.4  

    [800 rows x 5 columns]

</div>

</div>

<div class="cell markdown" id="Iuoh98kv0p3h">

# What groups to compare in the dataset

**G1 (daily mean temperature \| place = Ngong Ping) vs. G2 (daily mean
temperature \| place = Tai Mo Shan)**

</div>

<div class="cell markdown" id="zbqQ4IC5b9S9">

#First five records of each group

</div>

<div class="cell code" execution_count="3"
colab="{&quot;base_uri&quot;:&quot;https://localhost:8080/&quot;}"
id="qMidKjSe4gWU" outputId="3fe581ff-40e6-4ba0-f8c7-06c58897f422">

``` python
(df[df['place'] == 'Ngong Ping']['daily mean temperature']).head(5)
```

<div class="output execute_result" execution_count="3">

    0    14.1
    1    15.7
    2    17.5
    3    14.8
    4    13.3
    Name: daily mean temperature, dtype: float64

</div>

</div>

<div class="cell code" execution_count="4"
colab="{&quot;base_uri&quot;:&quot;https://localhost:8080/&quot;}"
id="M_y-hWLK4242" outputId="740201ee-17c7-476a-893e-a6832313c945">

``` python
(df[df['place'] == 'Tai Mo Shan']['daily mean temperature']).head(5)
```

<div class="output execute_result" execution_count="4">

    800    11.3
    801    12.8
    802    14.0
    803    11.8
    804     9.1
    Name: daily mean temperature, dtype: float64

</div>

</div>

<div class="cell markdown" id="GEDAWPFs4dF-">

#Information of Dataset

</div>

<div class="cell code" execution_count="5"
colab="{&quot;base_uri&quot;:&quot;https://localhost:8080/&quot;}"
id="dm52k_bzYhyA" outputId="1e44274a-c8c6-4601-9fb1-f76046bae94b">

``` python
print(df.info())
print()
print(df_c.info())
```

<div class="output stream stdout">

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1600 entries, 0 to 1599
    Data columns (total 5 columns):
     #   Column                  Non-Null Count  Dtype  
    ---  ------                  --------------  -----  
     0   年/Year                  1600 non-null   int64  
     1   月/Month                 1600 non-null   int64  
     2   日/Day                   1600 non-null   int64  
     3   place                   1600 non-null   object 
     4   daily mean temperature  1600 non-null   float64
    dtypes: float64(1), int64(3), object(1)
    memory usage: 62.6+ KB
    None

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 800 entries, 0 to 842
    Data columns (total 5 columns):
     #   Column                                 Non-Null Count  Dtype  
    ---  ------                                 --------------  -----  
     0   年/Year                                 800 non-null    int64  
     1   月/Month                                800 non-null    int64  
     2   日/Day                                  800 non-null    int64  
     3   daily mean temperature of Ngong Ping   800 non-null    float64
     4   daily mean temperature of Tai Mo Shan  800 non-null    float64
    dtypes: float64(2), int64(3)
    memory usage: 37.5 KB
    None

</div>

</div>

<div class="cell markdown" id="IbDD8EwNcrPl">

##Mean,Standard Deviation,minimun and maximun of each group

</div>

<div class="cell code" execution_count="6"
colab="{&quot;base_uri&quot;:&quot;https://localhost:8080/&quot;,&quot;height&quot;:143}"
id="qw99TBuBmhUk" outputId="e51c5c56-e627-470e-e867-d2c088265153">

``` python
df.groupby('place')['daily mean temperature'].agg(['count','mean', 'std', 'min', 'max'])
```

<div class="output execute_result" execution_count="6">

                 count       mean       std  min   max
    place                                             
    Ngong Ping     800  13.519500  3.735745  2.1  20.9
    Tai Mo Shan    800  11.333125  3.470861 -1.6  18.3

</div>

</div>

<div class="cell markdown" id="uezJcgKtebIg">

##The number of rows which G2>G1

</div>

<div class="cell code" execution_count="7"
colab="{&quot;base_uri&quot;:&quot;https://localhost:8080/&quot;}"
id="CZATJzcGHV8j" outputId="1efe6e90-1b96-4554-85b0-47db140a2fc7">

``` python
co = len(df_c[df_c['daily mean temperature of Ngong Ping'] < df_c['daily mean temperature of Tai Mo Shan']])
print('The number of rows which G2>G1:',co ,'/800')
```

<div class="output stream stdout">

    The number of rows which G2>G1: 45 /800

</div>

</div>

<div class="cell markdown" id="2MMeekQ81qbT">

#correlation of G1 & G2

</div>

<div class="cell code" execution_count="8"
colab="{&quot;base_uri&quot;:&quot;https://localhost:8080/&quot;}"
id="MfDt1rmr0Fji" outputId="ca86d94f-9a02-4872-f23c-64502c3114d9">

``` python
print(df_c['daily mean temperature of Ngong Ping'].corr(df_c['daily mean temperature of Tai Mo Shan']))
```

<div class="output stream stdout">

    0.9465213726326437

</div>

</div>

<div class="cell markdown" id="2xgaOvi7eone">

#Graphs

</div>

<div class="cell code" execution_count="9" id="xxbe8FsDlmB7">

``` python
import matplotlib.pyplot as plt
import seaborn as sns
plt.rcParams['figure.figsize'] = [5, 5]

sns.set()
```

</div>

<div class="cell markdown" id="HUAXRYHLeIQQ">

##Violinplot

</div>

<div class="cell code" execution_count="10"
colab="{&quot;base_uri&quot;:&quot;https://localhost:8080/&quot;,&quot;height&quot;:661}"
id="L7RLhrq5GEf0" outputId="66a7912d-9c09-4e08-b719-8dfd77328e54">

``` python
sns.violinplot(data=df,x='daily mean temperature', y='place')
plt.show()
sns.violinplot(data=df,x='daily mean temperature')
plt.show()
```

<div class="output display_data">

![](fe538d24fd44e47985b34c268d56ddd6487674a7.png)

</div>

<div class="output display_data">

![](588d69db7ce4eddaa65bb0ff04fdd61f5f0cfa85.png)

</div>

</div>

<div class="cell markdown" id="9-cO4lg5iEbb">

##Boxplot

</div>

<div class="cell code" execution_count="11"
colab="{&quot;base_uri&quot;:&quot;https://localhost:8080/&quot;,&quot;height&quot;:339}"
id="MeBFx8h8hi_y" outputId="6355a46f-4908-40b5-81a5-50e30fa7cea5">

``` python
sns.boxplot(data=df,x='daily mean temperature', y='place')
plt.show()
```

<div class="output display_data">

![](02d96d4462614d8f47bdd12dd707b84a84d204a8.png)

</div>

</div>

<div class="cell markdown" id="7X2z1580y9q8">

##Histogram

</div>

<div class="cell code" execution_count="12"
colab="{&quot;base_uri&quot;:&quot;https://localhost:8080/&quot;,&quot;height&quot;:339}"
id="9xZfuocVx48J" outputId="c060ea2a-d8a7-44e8-c3b8-f45336d4acbf">

``` python
sns.histplot(data=df,x='daily mean temperature', hue='place')
plt.show()
```

<div class="output display_data">

![](e7a74f92c667f7f7988eb267696129d1be175a95.png)

</div>

</div>
