

```python
import os
import pandas as pd
import mapreduce as mr
import csv
```


```python
!ls ./trips
```

    ltrips_short_exp123_wkdy_final_noXYcoord.txt
    ltrips_short_exp13_sat_final_noXYcoord.txt
    ltrips_short_exp13_sun_final_noXYcoord.txt


# Trip data
[relevant page](http://web.mta.info/mta/planning/data-nyc-travel.html)

![image.png](attachment:image.png)

Since this data is based on survey, MTA applied and validated weight. The description can be found in the report (pg. 46) <br>
But not fully understood! I just tried this. <br>
[Report](http://web.mta.info/mta/planning/data/NYC-Travel-Survey/NYCTravelSurvey.pdf)


```python
week = pd.read_csv('./trips/ltrips_short_exp123_wkdy_final_noXYcoord.txt', dtype=str)
```


```python
print('The number of total respondents: {}'.format(len(week)))
```

    The number of total respondents: 49167



```python
[(i,j) for i,j in zip(week.columns, range(len(week.columns)))]
```




    [('sampn', 0),
     ('perno', 1),
     ('tripno', 2),
     ('TRIP_ID', 3),
     ('otype', 4),
     ('dtype', 5),
     ('tpur', 6),
     ('dtime', 7),
     ('atime', 8),
     ('trip_sdate', 9),
     ('trip_edate', 10),
     ('O_COUNTY_STR', 11),
     ('D_COUNTY_STR', 12),
     ('Ptype', 13),
     ('daywk', 14),
     ('trans', 15),
     ('travy', 16),
     ('ntrav', 17),
     ('o_ntrav', 18),
     ('trtot', 19),
     ('ussob', 20),
     ('new_fare', 21),
     ('metro', 22),
     ('cash', 23),
     ('tmet', 24),
     ('o_tmet', 25),
     ('tumet', 26),
     ('o_tumet', 27),
     ('ppmet', 28),
     ('fare', 29),
     ('esub1', 30),
     ('esub2', 31),
     ('esub3', 32),
     ('esub4', 33),
     ('esub5', 34),
     ('esub6', 35),
     ('esub7', 36),
     ('esub8', 37),
     ('trip_count', 38),
     ('rectype', 39),
     ('s_dat', 40),
     ('s_dur', 41),
     ('ctfip', 42),
     ('incen', 43),
     ('stype', 44),
     ('lang', 45),
     ('hhnum', 46),
     ('hhveh', 47),
     ('ltele', 48),
     ('ctele', 49),
     ('cctel', 50),
     ('hinet', 51),
     ('hmrac', 52),
     ('o_hmrac', 53),
     ('ccmin', 54),
     ('ccinc', 55),
     ('cclin', 56),
     ('hcity', 57),
     ('hstat', 58),
     ('hzip', 59),
     ('hcnty', 60),
     ('puma', 61),
     ('MODE_G2', 62),
     ('MODE_G3', 63),
     ('MODE_G5', 64),
     ('MODE_G6', 65),
     ('MODE_G8', 66),
     ('MODE_G9', 67),
     ('MODE_G10', 68),
     ('PER_DAYTYPE', 69),
     ('TT_DAYTYPE', 70),
     ('MODE1', 71),
     ('MODE2', 72),
     ('MODE3', 73),
     ('MODE4', 74),
     ('MODE5', 75),
     ('MODE6', 76),
     ('MODE7', 77),
     ('MODE8', 78),
     ('MODE9', 79),
     ('MODE10', 80),
     ('MODE11', 81),
     ('MODE12', 82),
     ('MODE13', 83),
     ('MODE14', 84),
     ('MODE15', 85),
     ('MODE16', 86),
     ('O_Boro', 87),
     ('D_Boro', 88),
     ('HH_adults', 89),
     ('HH_workers', 90),
     ('R_BORO', 91),
     ('HH_SIZE', 92),
     ('HH_INC4', 93),
     ('HH_INC7', 94),
     ('HH_ETHNIC', 95),
     ('O_PURP', 96),
     ('O_NYTTAZ', 97),
     ('O_RTFMTAZ', 98),
     ('O_TRACT', 99),
     ('O_PUMA', 100),
     ('O_COUNTY', 101),
     ('D_PURP', 102),
     ('D_NYTTAZ', 103),
     ('D_RTFMTAZ', 104),
     ('D_TRACT', 105),
     ('D_PUMA', 106),
     ('D_COUNTY', 107),
     ('O_PURP_M', 108),
     ('D_PURP_M', 109),
     ('NSUB', 110),
     ('NNYBUS', 111),
     ('NCRAIL', 112),
     ('NOTHTRAN', 113),
     ('StopAreaNo', 114),
     ('HR_DEP', 115),
     ('HR_ARR', 116),
     ('TRIP_MIN', 117),
     ('EXP1_FINAL_WKD', 118),
     ('EXP21_FINAL_WKD', 119),
     ('EXP22_FINAL_WKD', 120),
     ('EXP31_FINAL_WKD', 121),
     ('EXP32_FINAL_WKD', 122),
     ('hmsex', 123),
     ('hmage', 124),
     ('SIRTOA_FLAG', 125),
     ('O_PLANDIST', 126),
     ('D_PLANDIST', 127)]




```python
sum(week['EXP32_FINAL_WKD'].astype(float).dropna())
```




    14308914.969999703




```python
week[['O_TRACT','D_TRACT']][week['O_TRACT'] == week['D_TRACT']]
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
      <th>O_TRACT</th>
      <th>D_TRACT</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>13</th>
      <td>36005017300.00</td>
      <td>36005017300.00</td>
    </tr>
    <tr>
      <th>34</th>
      <td>36005017300.00</td>
      <td>36005017300.00</td>
    </tr>
    <tr>
      <th>38</th>
      <td>36005005902.00</td>
      <td>36005005902.00</td>
    </tr>
    <tr>
      <th>47</th>
      <td>36005005901.00</td>
      <td>36005005901.00</td>
    </tr>
    <tr>
      <th>48</th>
      <td>36005005901.00</td>
      <td>36005005901.00</td>
    </tr>
    <tr>
      <th>49</th>
      <td>36005005901.00</td>
      <td>36005005901.00</td>
    </tr>
    <tr>
      <th>54</th>
      <td>36005005901.00</td>
      <td>36005005901.00</td>
    </tr>
    <tr>
      <th>62</th>
      <td>36005006500.00</td>
      <td>36005006500.00</td>
    </tr>
    <tr>
      <th>63</th>
      <td>36005006500.00</td>
      <td>36005006500.00</td>
    </tr>
    <tr>
      <th>64</th>
      <td>36005006500.00</td>
      <td>36005006500.00</td>
    </tr>
    <tr>
      <th>65</th>
      <td>36005006500.00</td>
      <td>36005006500.00</td>
    </tr>
    <tr>
      <th>67</th>
      <td>36061016400.00</td>
      <td>36061016400.00</td>
    </tr>
    <tr>
      <th>68</th>
      <td>36061016400.00</td>
      <td>36061016400.00</td>
    </tr>
    <tr>
      <th>73</th>
      <td>36061016400.00</td>
      <td>36061016400.00</td>
    </tr>
    <tr>
      <th>74</th>
      <td>36061016400.00</td>
      <td>36061016400.00</td>
    </tr>
    <tr>
      <th>86</th>
      <td>36005004700.00</td>
      <td>36005004700.00</td>
    </tr>
    <tr>
      <th>99</th>
      <td>36005004700.00</td>
      <td>36005004700.00</td>
    </tr>
    <tr>
      <th>100</th>
      <td>36005004700.00</td>
      <td>36005004700.00</td>
    </tr>
    <tr>
      <th>116</th>
      <td>36005019900.00</td>
      <td>36005019900.00</td>
    </tr>
    <tr>
      <th>117</th>
      <td>36005019900.00</td>
      <td>36005019900.00</td>
    </tr>
    <tr>
      <th>158</th>
      <td>36005006500.00</td>
      <td>36005006500.00</td>
    </tr>
    <tr>
      <th>180</th>
      <td>36005025500.00</td>
      <td>36005025500.00</td>
    </tr>
    <tr>
      <th>192</th>
      <td>36005023702.00</td>
      <td>36005023702.00</td>
    </tr>
    <tr>
      <th>193</th>
      <td>36005023702.00</td>
      <td>36005023702.00</td>
    </tr>
    <tr>
      <th>194</th>
      <td>36005023702.00</td>
      <td>36005023702.00</td>
    </tr>
    <tr>
      <th>195</th>
      <td>36005023702.00</td>
      <td>36005023702.00</td>
    </tr>
    <tr>
      <th>209</th>
      <td>36061030300.00</td>
      <td>36061030300.00</td>
    </tr>
    <tr>
      <th>210</th>
      <td>36061030300.00</td>
      <td>36061030300.00</td>
    </tr>
    <tr>
      <th>234</th>
      <td>36005005302.00</td>
      <td>36005005302.00</td>
    </tr>
    <tr>
      <th>235</th>
      <td>36005005302.00</td>
      <td>36005005302.00</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>48349</th>
      <td>36047054700.00</td>
      <td>36047054700.00</td>
    </tr>
    <tr>
      <th>48350</th>
      <td>36047054700.00</td>
      <td>36047054700.00</td>
    </tr>
    <tr>
      <th>48353</th>
      <td>36047054700.00</td>
      <td>36047054700.00</td>
    </tr>
    <tr>
      <th>48365</th>
      <td>36047052900.00</td>
      <td>36047052900.00</td>
    </tr>
    <tr>
      <th>48366</th>
      <td>36047052900.00</td>
      <td>36047052900.00</td>
    </tr>
    <tr>
      <th>48382</th>
      <td>36047055100.00</td>
      <td>36047055100.00</td>
    </tr>
    <tr>
      <th>48401</th>
      <td>36047089400.00</td>
      <td>36047089400.00</td>
    </tr>
    <tr>
      <th>48402</th>
      <td>36047089400.00</td>
      <td>36047089400.00</td>
    </tr>
    <tr>
      <th>48411</th>
      <td>36047114201.00</td>
      <td>36047114201.00</td>
    </tr>
    <tr>
      <th>48492</th>
      <td>36061000700.00</td>
      <td>36061000700.00</td>
    </tr>
    <tr>
      <th>48493</th>
      <td>36061000700.00</td>
      <td>36061000700.00</td>
    </tr>
    <tr>
      <th>48528</th>
      <td>36047042900.00</td>
      <td>36047042900.00</td>
    </tr>
    <tr>
      <th>48529</th>
      <td>36047042900.00</td>
      <td>36047042900.00</td>
    </tr>
    <tr>
      <th>48539</th>
      <td>36047042900.00</td>
      <td>36047042900.00</td>
    </tr>
    <tr>
      <th>48540</th>
      <td>36047042900.00</td>
      <td>36047042900.00</td>
    </tr>
    <tr>
      <th>48615</th>
      <td>34037373900.00</td>
      <td>34037373900.00</td>
    </tr>
    <tr>
      <th>48618</th>
      <td>36061014200.00</td>
      <td>36061014200.00</td>
    </tr>
    <tr>
      <th>48619</th>
      <td>36061014200.00</td>
      <td>36061014200.00</td>
    </tr>
    <tr>
      <th>48759</th>
      <td>36061027100.00</td>
      <td>36061027100.00</td>
    </tr>
    <tr>
      <th>48796</th>
      <td>36061024500.00</td>
      <td>36061024500.00</td>
    </tr>
    <tr>
      <th>48797</th>
      <td>36061024500.00</td>
      <td>36061024500.00</td>
    </tr>
    <tr>
      <th>48803</th>
      <td>36061027100.00</td>
      <td>36061027100.00</td>
    </tr>
    <tr>
      <th>48836</th>
      <td>36061009800.00</td>
      <td>36061009800.00</td>
    </tr>
    <tr>
      <th>48837</th>
      <td>36061009800.00</td>
      <td>36061009800.00</td>
    </tr>
    <tr>
      <th>48861</th>
      <td>36061025100.00</td>
      <td>36061025100.00</td>
    </tr>
    <tr>
      <th>49013</th>
      <td>36061000800.00</td>
      <td>36061000800.00</td>
    </tr>
    <tr>
      <th>49014</th>
      <td>36061000800.00</td>
      <td>36061000800.00</td>
    </tr>
    <tr>
      <th>49140</th>
      <td>36061026900.00</td>
      <td>36061026900.00</td>
    </tr>
    <tr>
      <th>49141</th>
      <td>36061026900.00</td>
      <td>36061026900.00</td>
    </tr>
    <tr>
      <th>49149</th>
      <td>36061027900.00</td>
      <td>36061027900.00</td>
    </tr>
  </tbody>
</table>
<p>3205 rows × 2 columns</p>
</div>



3205 trips has same origin and destination which is 6% of the total respondents.


```python
week_dif_OD = week[week['O_TRACT'] != week['D_TRACT']]
```


```python
len(week_dif_OD)
```




    45962




```python
# With OD same rows

reader = enumerate([(tract[:-3],1) for tract in week['O_TRACT'].astype(str)])
def mapper(_,o):
    yield(o)
def reducer(o,count):
    yield(o,sum(count))
o_tracts = list(mr.run(reader,mapper,reducer))

reader = enumerate([(tract[:-3],1) for tract in week['D_TRACT'].astype(str)])
def mapper(_,o):
    yield(o)
def reducer(o,count):
    yield(o,sum(count))
d_tracts = list(mr.run(reader,mapper,reducer))
```


```python
# Without OD same rows

reader = enumerate([(tract[:-3],1) for tract in week_dif_OD['O_TRACT'].astype(str)])
def mapper(_,o):
    yield(o)
def reducer(o,count):
    yield(o,sum(count))
o_tracts = list(mr.run(reader,mapper,reducer))

reader = enumerate([(tract[:-3],1) for tract in week_dif_OD['D_TRACT'].astype(str)])
def mapper(_,o):
    yield(o)
def reducer(o,count):
    yield(o,sum(count))
d_tracts = list(mr.run(reader,mapper,reducer))
```


```python
# with open('eggs.csv', 'w', newline='') as csvfile:
#     spamwriter = csv.writer(csvfile, delimiter=' ',
#                             quotechar='|', quoting=csv.QUOTE_MINIMAL)
#     for i in o_tracts:
#         spamwriter.writerow(i)

# with open('spam.csv', 'w', newline='') as csvfile:
#     spamwriter = csv.writer(csvfile, delimiter=' ',
#                             quotechar='|', quoting=csv.QUOTE_MINIMAL)
#     for i in d_tracts:
#         spamwriter.writerow(i)
```


```python
trip_o = pd.DataFrame(o_tracts, columns=['tracts','trip_o'])
trip_d = pd.DataFrame(d_tracts, columns=['tracts','trip_d'])
```


```python
display(trip_o.head(), trip_d.head())
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
      <th>tracts</th>
      <th>trip_o</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>11001005900</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>11001008600</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>19061000100</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>25019950100</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>25019950400</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



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
      <th>tracts</th>
      <th>trip_d</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td></td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>11001008600</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>25019950400</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>25027759100</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>34001002400</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>


# Census Tracts

## Census Tract Code Format
![image.png](attachment:image.png)
* census tract code
* state + county + tract
Counties in NYC <br>
[county code in NYC](https://www2.census.gov/geo/docs/reference/codes/files/st36_ny_cou.txt) <br>
State Code of NYC : 36 <br>
* Manhattan - New York County (061)
* Bronx - Bronx County (005)
* Brooklyn - Kings County (047)
* Queens - Queens County (081)
* Staten Island - Richmond County (085)


```python
ct2000 = pd.read_csv('nyct2000wi.csv', dtype=str)
```


```python
ct2000.head()
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
      <th>BoroCode</th>
      <th>BoroName</th>
      <th>CTLabel</th>
      <th>the_geom</th>
      <th>CT2000</th>
      <th>BoroCT2000</th>
      <th>CDEligibil</th>
      <th>NTACode</th>
      <th>NTANAme</th>
      <th>PUMA</th>
      <th>Shape_Leng</th>
      <th>Shape_Area</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Manhattan</td>
      <td>98</td>
      <td>MULTIPOLYGON (((-73.96432543478758 40.75638153...</td>
      <td>009800</td>
      <td>1009800</td>
      <td>I</td>
      <td>MN19</td>
      <td>Turtle Bay-East Midtown</td>
      <td>3808</td>
      <td>5534.20005976</td>
      <td>1906016.374</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Manhattan</td>
      <td>100</td>
      <td>MULTIPOLYGON (((-73.96802436915851 40.75957814...</td>
      <td>010000</td>
      <td>1010000</td>
      <td>I</td>
      <td>MN19</td>
      <td>Turtle Bay-East Midtown</td>
      <td>3808</td>
      <td>5692.16866575</td>
      <td>1860938.35958</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>Manhattan</td>
      <td>190</td>
      <td>MULTIPOLYGON (((-73.94505127984516 40.80259859...</td>
      <td>019000</td>
      <td>1019000</td>
      <td>E</td>
      <td>MN11</td>
      <td>Central Harlem South</td>
      <td>3803</td>
      <td>4231.8265884</td>
      <td>1117371.65978</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>Manhattan</td>
      <td>206</td>
      <td>MULTIPOLYGON (((-73.93580780201182 40.80949763...</td>
      <td>020600</td>
      <td>1020600</td>
      <td>E</td>
      <td>MN03</td>
      <td>Central Harlem North-Polo Grounds</td>
      <td>3803</td>
      <td>5176.87315098</td>
      <td>1602693.82096</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>Manhattan</td>
      <td>217.02</td>
      <td>MULTIPOLYGON (((-73.9470345118215 40.815829904...</td>
      <td>021702</td>
      <td>1021702</td>
      <td>E</td>
      <td>MN03</td>
      <td>Central Harlem North-Polo Grounds</td>
      <td>3803</td>
      <td>3338.290909</td>
      <td>446993.881638</td>
    </tr>
  </tbody>
</table>
</div>




```python
ct2000.BoroName.unique()
```




    array(['Manhattan', 'Bronx', 'Queens', 'Brooklyn', 'Staten Island'],
          dtype=object)




```python
ct2000['cdct'][ct2000['BoroName']=='Manhattan'] = '36061' + ct2000['CT2000']
ct2000['cdct'][ct2000['BoroName']=='Bronx'] = '36005' + ct2000['CT2000']
ct2000['cdct'][ct2000['BoroName']=='Brooklyn'] = '36047' + ct2000['CT2000']
ct2000['cdct'][ct2000['BoroName']=='Queens'] = '36081' + ct2000['CT2000']
ct2000['cdct'][ct2000['BoroName']=='Staten Island'] = '36085' + ct2000['CT2000']
```


```python
ct2000[:2]
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
      <th>BoroCode</th>
      <th>BoroName</th>
      <th>CTLabel</th>
      <th>the_geom</th>
      <th>CT2000</th>
      <th>BoroCT2000</th>
      <th>CDEligibil</th>
      <th>NTACode</th>
      <th>NTANAme</th>
      <th>PUMA</th>
      <th>Shape_Leng</th>
      <th>Shape_Area</th>
      <th>cdct</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Manhattan</td>
      <td>98</td>
      <td>MULTIPOLYGON (((-73.96432543478758 40.75638153...</td>
      <td>009800</td>
      <td>1009800</td>
      <td>I</td>
      <td>MN19</td>
      <td>Turtle Bay-East Midtown</td>
      <td>3808</td>
      <td>5534.20005976</td>
      <td>1906016.374</td>
      <td>36061009800</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Manhattan</td>
      <td>100</td>
      <td>MULTIPOLYGON (((-73.96802436915851 40.75957814...</td>
      <td>010000</td>
      <td>1010000</td>
      <td>I</td>
      <td>MN19</td>
      <td>Turtle Bay-East Midtown</td>
      <td>3808</td>
      <td>5692.16866575</td>
      <td>1860938.35958</td>
      <td>36061010000</td>
    </tr>
  </tbody>
</table>
</div>




```python
print(ct2000.cdct.dtype, trip_o.tracts.dtype, trip_d.tracts.dtype )
print(trip_o.trip_o.dtype, trip_d.trip_d.dtype)
```

    object object object
    int64 int64



```python
mrg = pd.merge(ct2000, trip_o, how='inner', left_on='cdct', right_on='tracts')
mrg = pd.merge(mrg, trip_d, how='inner', left_on='cdct', right_on='tracts')
```


```python
print(len(mrg))
display(mrg.head())
```

    2120



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
      <th>BoroCode</th>
      <th>BoroName</th>
      <th>CTLabel</th>
      <th>the_geom</th>
      <th>CT2000</th>
      <th>BoroCT2000</th>
      <th>CDEligibil</th>
      <th>NTACode</th>
      <th>NTANAme</th>
      <th>PUMA</th>
      <th>Shape_Leng</th>
      <th>Shape_Area</th>
      <th>cdct</th>
      <th>tracts_x</th>
      <th>trip_o</th>
      <th>tracts_y</th>
      <th>trip_d</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Manhattan</td>
      <td>98</td>
      <td>MULTIPOLYGON (((-73.96432543478758 40.75638153...</td>
      <td>009800</td>
      <td>1009800</td>
      <td>I</td>
      <td>MN19</td>
      <td>Turtle Bay-East Midtown</td>
      <td>3808</td>
      <td>5534.20005976</td>
      <td>1906016.374</td>
      <td>36061009800</td>
      <td>36061009800</td>
      <td>72</td>
      <td>36061009800</td>
      <td>74</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Manhattan</td>
      <td>100</td>
      <td>MULTIPOLYGON (((-73.96802436915851 40.75957814...</td>
      <td>010000</td>
      <td>1010000</td>
      <td>I</td>
      <td>MN19</td>
      <td>Turtle Bay-East Midtown</td>
      <td>3808</td>
      <td>5692.16866575</td>
      <td>1860938.35958</td>
      <td>36061010000</td>
      <td>36061010000</td>
      <td>145</td>
      <td>36061010000</td>
      <td>140</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>Manhattan</td>
      <td>190</td>
      <td>MULTIPOLYGON (((-73.94505127984516 40.80259859...</td>
      <td>019000</td>
      <td>1019000</td>
      <td>E</td>
      <td>MN11</td>
      <td>Central Harlem South</td>
      <td>3803</td>
      <td>4231.8265884</td>
      <td>1117371.65978</td>
      <td>36061019000</td>
      <td>36061019000</td>
      <td>36</td>
      <td>36061019000</td>
      <td>33</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>Manhattan</td>
      <td>206</td>
      <td>MULTIPOLYGON (((-73.93580780201182 40.80949763...</td>
      <td>020600</td>
      <td>1020600</td>
      <td>E</td>
      <td>MN03</td>
      <td>Central Harlem North-Polo Grounds</td>
      <td>3803</td>
      <td>5176.87315098</td>
      <td>1602693.82096</td>
      <td>36061020600</td>
      <td>36061020600</td>
      <td>9</td>
      <td>36061020600</td>
      <td>9</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>Manhattan</td>
      <td>217.02</td>
      <td>MULTIPOLYGON (((-73.9470345118215 40.815829904...</td>
      <td>021702</td>
      <td>1021702</td>
      <td>E</td>
      <td>MN03</td>
      <td>Central Harlem North-Polo Grounds</td>
      <td>3803</td>
      <td>3338.290909</td>
      <td>446993.881638</td>
      <td>36061021702</td>
      <td>36061021702</td>
      <td>13</td>
      <td>36061021702</td>
      <td>13</td>
    </tr>
  </tbody>
</table>
</div>



```python
mrg.to_csv('od_trip_ct2008.csv')
```

# aggregating with population data


```python
!wget -O pop_ct.csv https://data.cityofnewyork.us/api/views/kc6e-jm93/rows.csv?accessType=DOWNLOAD
```

    --2019-02-27 08:48:28--  https://data.cityofnewyork.us/api/views/kc6e-jm93/rows.csv?accessType=DOWNLOAD
    Resolving data.cityofnewyork.us (data.cityofnewyork.us)... 52.206.68.26, 52.206.140.199, 52.206.140.205
    Connecting to data.cityofnewyork.us (data.cityofnewyork.us)|52.206.68.26|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: unspecified [text/csv]
    Saving to: ‘pop_ct.csv’
    
    pop_ct.csv              [ <=>                ] 132.31K  --.-KB/s    in 0.002s  
    
    Last-modified header invalid -- time-stamp ignored.
    2019-02-27 08:48:29 (57.7 MB/s) - ‘pop_ct.csv’ saved [135483]
    



```python
pop2000 = pd.read_csv('pop_ct.csv', dtype=str)
pop2000 = pop2000[pop2000['Year'] == '2000']
```


```python
pop2000.Year.unique()
```




    array(['2000'], dtype=object)




```python
pop2000.head()
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
      <th>Borough</th>
      <th>Year</th>
      <th>FIPS County Code</th>
      <th>DCP Borough Code</th>
      <th>Census Tract</th>
      <th>Population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bronx</td>
      <td>2000</td>
      <td>005</td>
      <td>2</td>
      <td>000100</td>
      <td>12780</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Bronx</td>
      <td>2000</td>
      <td>005</td>
      <td>2</td>
      <td>000200</td>
      <td>3545</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bronx</td>
      <td>2000</td>
      <td>005</td>
      <td>2</td>
      <td>000400</td>
      <td>3314</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Bronx</td>
      <td>2000</td>
      <td>005</td>
      <td>2</td>
      <td>001600</td>
      <td>5237</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bronx</td>
      <td>2000</td>
      <td>005</td>
      <td>2</td>
      <td>001900</td>
      <td>1584</td>
    </tr>
  </tbody>
</table>
</div>




```python
pop2000['cdct'] = '36' + pop2000['FIPS County Code'] + pop2000['Census Tract']
```


```python
pop2000.head()
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
      <th>Borough</th>
      <th>Year</th>
      <th>FIPS County Code</th>
      <th>DCP Borough Code</th>
      <th>Census Tract</th>
      <th>Population</th>
      <th>cdct</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bronx</td>
      <td>2000</td>
      <td>005</td>
      <td>2</td>
      <td>000100</td>
      <td>12780</td>
      <td>36005000100</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Bronx</td>
      <td>2000</td>
      <td>005</td>
      <td>2</td>
      <td>000200</td>
      <td>3545</td>
      <td>36005000200</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bronx</td>
      <td>2000</td>
      <td>005</td>
      <td>2</td>
      <td>000400</td>
      <td>3314</td>
      <td>36005000400</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Bronx</td>
      <td>2000</td>
      <td>005</td>
      <td>2</td>
      <td>001600</td>
      <td>5237</td>
      <td>36005001600</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bronx</td>
      <td>2000</td>
      <td>005</td>
      <td>2</td>
      <td>001900</td>
      <td>1584</td>
      <td>36005001900</td>
    </tr>
  </tbody>
</table>
</div>




```python
mrg = pd.merge(mrg, pop2000, how='inner', on='cdct')
```


```python
mrg.columns
```




    Index(['BoroCode', 'BoroName', 'CTLabel', 'the_geom', 'CT2000', 'BoroCT2000',
           'CDEligibil', 'NTACode', 'NTANAme', 'PUMA', 'Shape_Leng', 'Shape_Area',
           'cdct', 'tracts_x', 'trip_o', 'tracts_y', 'trip_d', 'Borough', 'Year',
           'FIPS County Code', 'DCP Borough Code', 'Census Tract', 'Population'],
          dtype='object')




```python
mrg = mrg[['BoroName', 'the_geom', 'cdct', 'trip_o', 'trip_d', 'Year', 'Population']]
```


```python
mrg.head()
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
      <th>BoroName</th>
      <th>the_geom</th>
      <th>cdct</th>
      <th>trip_o</th>
      <th>trip_d</th>
      <th>Year</th>
      <th>Population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Manhattan</td>
      <td>MULTIPOLYGON (((-73.96432543478758 40.75638153...</td>
      <td>36061009800</td>
      <td>72</td>
      <td>74</td>
      <td>2000</td>
      <td>7066</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Manhattan</td>
      <td>MULTIPOLYGON (((-73.96802436915851 40.75957814...</td>
      <td>36061010000</td>
      <td>145</td>
      <td>140</td>
      <td>2000</td>
      <td>1822</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Manhattan</td>
      <td>MULTIPOLYGON (((-73.94505127984516 40.80259859...</td>
      <td>36061019000</td>
      <td>36</td>
      <td>33</td>
      <td>2000</td>
      <td>1818</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Manhattan</td>
      <td>MULTIPOLYGON (((-73.93580780201182 40.80949763...</td>
      <td>36061020600</td>
      <td>9</td>
      <td>9</td>
      <td>2000</td>
      <td>2310</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Manhattan</td>
      <td>MULTIPOLYGON (((-73.93669078722161 40.83719324...</td>
      <td>36061024900</td>
      <td>6</td>
      <td>6</td>
      <td>2000</td>
      <td>1150</td>
    </tr>
  </tbody>
</table>
</div>




```python
mrg['trip_o'] - mrg['trip_d']
```




    0       -2
    1        5
    2        3
    3        0
    4        0
    5        6
    6       -6
    7        0
    8        1
    9        3
    10       1
    11       3
    12       2
    13     -11
    14       2
    15      -1
    16      -6
    17       1
    18       1
    19      -1
    20       0
    21       0
    22       0
    23       0
    24       0
    25       0
    26       0
    27       0
    28       0
    29       0
            ..
    1821     0
    1822    -2
    1823     1
    1824    -1
    1825     0
    1826     0
    1827    -7
    1828     0
    1829     1
    1830     0
    1831     0
    1832     0
    1833     0
    1834    -5
    1835    -1
    1836     0
    1837     0
    1838     6
    1839    -1
    1840    -1
    1841     1
    1842     0
    1843     0
    1844     1
    1845    -1
    1846     0
    1847     0
    1848     0
    1849     0
    1850     0
    Length: 1851, dtype: int64




```python
mrg.to_csv('trip_with_pop.csv', index=False)
```

![image.png](attachment:image.png)

Figure 0. Origin Data

![image.png](attachment:image.png)

Figure 0. Destination data (Quantile)
