# shreya_JJJ
from collections import defaultdict
import pandas as pd
import csv
import copy
from csv import DictReader

with open('Data/Form2.csv',encoding = "ISO-8859-1") as fh:
    data = csv.DictReader(fh)
    
    occGroup  = {}
    workWomen = {}
    abor = {}
    aborWomen = {}
    aborMen = {}

    # Split the data depending on OCCGROUP
    for row in data:
        if row["OCCGROUP"] in occGroup:
            occGroup[row["OCCGROUP"]].append(row)
        else:
            occGroup[row["OCCGROUP"]] = [row]

# Add to the list depending on the category
for i in occGroup:
        count = 0
        while count != len(occGroup[i]):
            if i in workWomen:
                workWomen[i].append(occGroup[i][count]["ALLWOMENCOUNT"])
                abor[i].append(occGroup[i][count]["ABORIGALLCOUNT"])
                aborWomen[i].append(occGroup[i][count]["ABORIGWOMENCOUNT"])
                aborMen[i].append(occGroup[i][count]["ABORIGMENCOUNT"])
                count+=1
           
            else:
                workWomen[i] = [occGroup[i][count]["ALLWOMENCOUNT"]]
                abor[i] = [occGroup[i][count]["ABORIGALLCOUNT"]]
                aborWomen[i] = [occGroup[i][count]["ABORIGWOMENCOUNT"]]
                aborMen[i] = [occGroup[i][count]["ABORIGMENCOUNT"]]

# Calculate totals
total = 0
for x in workWomen:
    for y in workWomen[x]:
        if len(y)!= 0:
            total += int(y)
        workWomen[x] = total   

total2 = 0
for x in abor:
    for y in abor[x]:
        if len(y)!= 0:
            total2 += int(y)
        abor[x] = total2 

total3 = 0
for x in aborWomen:
    for y in aborWomen[x]:
        if len(y)!= 0:
            total3 += int(y)
        aborWomen[x] = total3

total4 = 0
for x in aborMen:
    for y in aborMen[x]:
        if len(y)!= 0:
            total4 += int(y)
        aborMen[x] = total4

addNameWomen = {"Name":'Women by Occupational Group, 2019'}
addNameWomen.update(workWomen)

addNameAbor = {"Name":'Aboriginals by Occupational Group, 2019'}
addNameAbor.update(abor)

addNameWomenAbor = {"Name":'Women Aboriginals by Occupational Group, 2019'}
addNameWomenAbor.update(aborWomen)

addNameMenAbor = {"Name":'Men Aboriginals by Occupational Group, 2019'}
addNameMenAbor.update(aborMen)


df = pd.DataFrame.from_dict(addNameWomen,orient='index').T
df = df.append(pd.DataFrame.from_dict(addNameAbor,orient='index').T)
df = df.append(pd.DataFrame.from_dict(addNameWomenAbor,orient='index').T)
df = df.append(pd.DataFrame.from_dict(addNameMenAbor,orient='index').T)
df
