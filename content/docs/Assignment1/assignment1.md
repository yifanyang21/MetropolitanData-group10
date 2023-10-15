---
title: Assignment 1  –  Water 
next: /
toc: false
type: about
---


## Background:

Paris is hosting the Paralympics in 2024. One of the events at the Paralympics is an open water swimming event in the Seine (apparently the water is clean or so the French say). Since the City of Amsterdam thinks it is better than Paris, they want to host an event before the Paralympics, snubbing the Parisians. The idea is to host a 5km. open water swimming event through the canals of Amsterdam. You are asked by the municipality of Amsterdam to advise on the feasibility of the event from the perspective of the safety of the partaking athletes from an environmental perspective. The event is going to be hosted in May. 

---

## Our Solutions:

To convey a proper opinion on the feasibility of the event, our group would need data about the quality of the water in the canals and the most populous routes for water transport and canal boats. The water quality data would need to include escherichia coli (EC) and intestinal entercoocci (IE) levels because this would tell us the sewage level at the location. The water transport and canal boat data would tell us where the busiest routes are, so therefore where to most likely avoid. However, the water quality of the canals will be a priority to impacting routes. Oxygen, temperature, and turbidity are also good indicators of water quality, so data sets with this information would be helpful in conveying the feasibility.  

 

The following table is a list of relevant data sets we found and the format they are stored in. In the table we answer the questions about file format, data type, the readability and the required python library.  

<table>
<thead>
<tr>
<th style="text-align:left">Name</th>
<th style="text-align:left" width="200">Source/Link</th>
<th style="text-align:left">File format</th>
<th style="text-align:left">Data type</th>
<th style="text-align:left">Scale</th>
<th style="text-align:left">Python library</th>
<th style="text-align:left">Readable or not</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left">Measurement results surface water quality research, 2019</td>
<td style="text-align:left" width="200">https://onderzoek.amsterdam.nl/dataset/water-in-amsterdam</td>
<td style="text-align:left">Excel file</td>
<td style="text-align:left">Numerical</td>
<td style="text-align:left">Municipal scale (Municipality of Amsterdam)</td>
<td style="text-align:left">"pandas"" "openpyxl" library, use the read_excel() function.</td>
<td style="text-align:left">Yes</td>
</tr>
<tr>
<td style="text-align:left">Mapping our water: water visibility</td>
<td style="text-align:left" width="200">https://www.waternet.nl/ons-water/oppervlaktewater/water-in-kaart/</td>
<td style="text-align:left">Online map</td>
<td style="text-align:left">Geodata (points, with numerical data)</td>
<td style="text-align:left">Municipal scale (Municipality of Amsterdam)</td>
<td style="text-align:left">(maybe we can use “requests” library to make HTTP requests and retrieve raw data)</td>
<td style="text-align:left">Yes</td>
</tr>
<tr>
<td style="text-align:left">Mapping our water: Salt</td>
<td style="text-align:left" width="200">https://www.waternet.nl/ons-water/oppervlaktewater/water-in-kaart/</td>
<td style="text-align:left">Online map</td>
<td style="text-align:left">Geodata (points, with numerical data)</td>
<td style="text-align:left">Municipal scale (Municipality of Amsterdam)</td>
<td style="text-align:left">(maybe we can use “requests” library to make HTTP requests and retrieve raw data)</td>
<td style="text-align:left">Yes</td>
</tr>
<tr>
<td style="text-align:left">Mapping our water: Oxygen</td>
<td style="text-align:left" width="200">https://www.waternet.nl/ons-water/oppervlaktewater/water-in-kaart/</td>
<td style="text-align:left">Online map</td>
<td style="text-align:left">Geodata (points, with numerical data)</td>
<td style="text-align:left">Municipal scale (Municipality of Amsterdam)</td>
<td style="text-align:left">(maybe we can use “requests” library to make HTTP requests and retrieve raw data)</td>
<td style="text-align:left">Yes</td>
</tr>
<tr>
<td style="text-align:left">zwemwater.nl</td>
<td style="text-align:left" width="200">https://www.zwemwater.nl/home</td>
<td style="text-align:left">Online map</td>
<td style="text-align:left">Geodata (points, with numerical data)</td>
<td style="text-align:left">National scale (Netherlands)</td>
<td style="text-align:left">(maybe we can use “requests” library to make HTTP requests and retrieve raw data)</td>
<td style="text-align:left">Yes</td>
</tr>
<tr>
<td style="text-align:left">Boarding and disembarking points & berths passenger vessels</td>
<td style="text-align:left" width="200">https://maps.amsterdam.nl/varen/</td>
<td style="text-align:left">Online map</td>
<td style="text-align:left">Geodata (points with symbolic labels)</td>
<td style="text-align:left">Municipal scale (Municipality of Amsterdam)</td>
<td style="text-align:left">(maybe we can use “requests” library to make HTTP requests and retrieve raw data)</td>
<td style="text-align:left">Yes</td>
</tr>
</tbody>
</table>

However, we were unable to find more extensive and precise water quality testing data for the Amsterdam canals, and could only extrapolate and speculate based on data from a few monitoring points. Having such data would enable us to make more targeted recommendations.

---


## Read and print by Python

The following program is to read and print the data from <i>Measurement results surface water quality research, 2019</i>

```python
#pip install openpyxl

import openpyxl

workbook = openpyxl.load_workbook('Meetresultaten kwaliteitsonderzoek oppervlaktewater.xlsx')
sheet = workbook.active

for row in sheet.iter_rows(values_only=True):
    list_row = list(row)
    print(list_row)
 
```
or

```python
#pip install pandas

import pandas as pd

df = pd.read_excel('Meetresultaten kwaliteitsonderzoek oppervlaktewater.xlsx')

print(df)
```