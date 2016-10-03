
# OpenStreetMap Data Wrangling

## Map Area

Singapore, Singapore
- http://overpass-api.de/api/map?bbox=103.6515,1.2177,104.1071,1.4737

Singapore was chosen as it is my hometown. A manually selected box of the Singapore area was chosen (uncompressed size of file = 161.1MB). This was used instead of the predetermined Singapore area from Mapzen as it was a purely Singapore territory as opposed to the area provided in Mapzen which included Indonesian and Malaysian territories.

## Problems Encountered in Maps

A sample osm file was created for initial exploration. The original code provided in class was used and modified, if necessary, provided a first glance into the data. Problems encountered in the data set can be categorised broadly into the following 5 areas.

#### 1. Wrong country included in data
#### 2. Wrong cities
#### 3. Erroneous street types
#### 4. Wrong zipcodes
#### 5. Miscellaneous errors

### Problem 1: Wrong country included in data

Even though the data set was obtained from a manually selected box of coordinates (well-within territorial limits of Singapore), there were still data with country entry as Malaysia. This was a simple fix, through the modification of the original code for auditing street names to audit "country" entry. Turned out that there were only 2 types of country entries: "SG" and "MY". The "id" of such entries were collected into a set and omitted when writing to csv file.

```py
def is_wrong_country(elem):
    return (elem.attrib['k'] == "addr:country") and (elem.attrib['v'] != 'SG')

def auditcountry(osmfile):
    non_sgp = set()
    for elem in get_element(osm_file):

        if elem.tag == "node" or elem.tag == "way":
            iden = elem.get('id')
            for tag in elem.iter("tag"):
                if is_wrong_country(tag):
                    non_sgp.add(iden)
                    
    return non_sgp
```

### Problem 2: Wrong cities included in data

Thinking that only Singapore data remained, we proceeded to audit the street names. However, Malaysian street names continued to appear in the data set. Therefore, it was decided that "city" entries had to be audited as well. 

After auditing, there were over 1,000 entries with non-Singapore city entries. Since some of these entries are actual Singapore data set with erroneous "city" value, a manual city-mapping dictionary had to be created to correct the city value.

```py
defaultdict(set,
            {'01-169 Singapore': {'3066813432'},
             'Ang Mo Kio': {'99862506'},
             'Changi Village': {'4074595871'},
             'Holland Village': {'3629227220'},
             'JB': {'3678574101'},
             'Johor Bahru': {'396940848',
              '390224852',
              '390224853',
              '390224850',
              ...},
             'Nusajaya': {'192574776', '308040645', '308040682', '308040695'},
             'Pasir Gudang, Johor': {'287211259'},
             'Punggol': {'3823826493'},
             'Sembawang': {'167189988', '62204073', '67327914'},
             'Tanjung Puteri': {'201941456'},
             'Woodlands Spectrum II': {'171493307'},
             'singapore': {'152431988',
              '155986468',
              '3461120954',
              '409015811'}})
              ```

To prevent the case of wrongly placed "correct data" (i.e. correct city value entered into street name, correct street name entered into city field, etc), entire tag of such erroneous entries was printed out to visually inspect the entry.

From the two examples below, we see that for the first entry, city was erroneously labelled as "Ang Mo Kio" instead of "Singapore". However, the information (town) "Ang Mo Kio" was captured in the street name, so it was safe to replace city with "Singapore" directly.

For the second example, "Woodlands Spectrum II" should be the building name, and building/house number "207" occupied 2 entries "housenumber" and "name". Hence, manual correction is required for such entries.


- 99862506 addr:city --- Ang Mo Kio
- 99862506 addr:housenumber --- Blk 233
- 99862506 addr:postcode --- 560233
- 99862506 addr:street --- Ang Mo Kio avenue 3
- 99862506 building --- residential
- 99862506 building:levels --- 01-1180
- 99862506 name --- 233



- 171493307 addr:city --- Woodlands Spectrum II
- 171493307 addr:housenumber --- 207
- 171493307 addr:postcode --- 738958
- 171493307 addr:street --- Woodlands Avenue 9
- 171493307 building --- industrial
- 171493307 building:levels --- 5
- 171493307 name --- Block 207

After filtering the entries to be corrected, the rest of entries with erroneous city were collected into another set and unioned with the set of wrong country entries, to be omitted when writing to the csv files in the "shape_element" function.

### Problem 3: Wrong street name

The original code from class to audit street name was used for initial exploration of the street names and to populate the street name map for cleaning. It was discovered that Singapore street names differ from that of USA in that they can take different forms:

#### Street type first (Type A)

E.g. "Lorong Ah Soo", "Jalan Bkt Ho Swee", where "Lorong" and "Jalan" are the street types

For Type A, the list is rather small (only 4 street types like this), so a list of standard Type A was created and a "front_map" dictionary was created for the erroneous permutations of these street types. Regex was used to fish out the first word of the street name to match and correct the street names.

##### Problem with Type A

A problem arose later which could be traced back to type A street types. While generating the first set of CSV files, it was found that non-Singapore entries continued to appear within the data set, even after filtering non-Singapore "country" and "city" entries.

It was found that, at least partially, some of those entries "slipped through" the audit process because they had the same "standard" street types at the front, i.e. Jalan Johan (a Johor Bahru, Malaysia street name and Jalan is a very common street type in Malaysia). Therefore, all street names with street types at the front were printed for visual inspection and crossed referenced manually with "gold standards" from www.streetdirectory.com to verify if these streets are indeed within Singapore (a relatively short process due to local knowledge). Non-Singapore entries were identified by their IDs and added to the non-Singapore set to be filtered out before writing to CSV files.

The entire tag of such erroneous entries were visually inspected for possible reasons for their inclusion in the dataset, but except for missing city and country fields, no other reasonable hypotheses could be established. Latitude and longitudes values were not checked.

#### Numbered street type (Type B)

E.g. "Clementi Avenue 1", "Jurong West Street 65"

For Type B, regex was used to match street names ending in the form of "word_space_digit(s)". Thereafter, the street type was audited and corrected using the same method for type C.

#### Street type last (Type C)

E.g. "Orchard Road", "Libra Drive"

Type C is similar to that in USA, so the same type of audit and correction were used.

### Problem 4: Wrong zipcodes

Zipcodes were audited using regex as Singapore zipcodes conform to a 6-digit format. Mostly errors were typographic (i.e. 5 digits instead of 6, extra space in the middle) while the rest had a string "< different >". Similar to auditing for "city" field, in order to ensure no important information was overwritten, entire tag for each entry was printed for visual inspection.

"Gold standard" was defined as zipcodes generated from www.streetdirectory.com.sg as it is the organisation which has been producing Singapore maps for decades.

"ID" was used as the identifier and "gold standard" zipcodes were manually entered into a mapping dictionary. However, since IDs are non-unique between nodes and ways, IDs from nodes and ways had to be handled separately in different sets.

### Problem 5: Miscellaneous errors

During the process of exploring, auditing and cleaning the above 4 problems, some miscellaneous errors were discovered. One example was information entered into the wrong field, from problem 2. As such errors are one-off and non-systematic, specific corrections had to be applied to make good the data. IDs of such entries were recorded and their specific erroneous fields were replaced, via mapping, with corrected values.

## Data Overview

### File sizes

```
singapore.osm --- 161.1MB
sgmap.db --- 96.6MB
nodes.csv --- 57.5MB
node_tags.csv --- 12.7MB
ways.csv --- 5.7MB
way_tags.csv --- 8.5MB
way_nodes.csv --- 20.5MB
```

### Number of nodes

```sql
sqlite> select count(*) from nodes;
707212
```

### Number of ways

```sql
sqlite> select count(*) from ways;
97179
```

### Number of unique users

```sql
sqlite> SELECT COUNT(DISTINCT(uid))
   ...> FROM nodes;
956
sqlite> SELECT COUNT(DISTINCT(uid))
   ...> FROM ways;
778
sqlite> SELECT COUNT(DISTINCT(a.uid))
   ...> FROM (SELECT uid FROM nodes UNION ALL SELECT uid FROM ways) a;
1122
```

### Top 10 contributing users

```sql
sqlite> SELECT a.user, count(*) as num
   ...> FROM (SELECT user FROM nodes UNION ALL SELECT user FROM ways) a
   ...> GROUP BY a.user
   ...> ORDER BY num DESC
   ...> LIMIT 10;

JaLooNz|266644
cboothroyd|69802
rene78|57881
jaredc|29182
zomgvivian|24873
singastreet|19177
fusionstream|18415
berjaya|17373
Phoony|16501
matx17|15973
```

### Average number of entries per user

```sql
sqlite> SELECT avg(b.num)
   ...> FROM (SELECT count(*) as num
   ...> FROM (SELECT user FROM nodes UNION ALL SELECT user FROM ways) a
   ...> GROUP BY a.user) b;

716.926024955437
```

### Number of users with only 1 entry

```sql
sqlite> SELECT count(*)
   ...> FROM (SELECT a.user, count(*) as num
   ...> FROM (SELECT user FROM nodes UNION ALL SELECT user FROM ways) a
   ...> GROUP BY a.user
   ...> HAVING num = 1) b;

267
```

Top 10 contributors to nodes and ways account for over 66.61%. This is much lower than the sample report in Charlotte, NC, where top 10 contributors accounted for over 90% of entries. Contributions to Singapore OSM dataset is much less skewed than that in Charlotte, NC.

## Additional Data Exploration

### Top 15 types of shops in Singapore

```sql
sqlite> SELECT value, count(*) as num 
   ...> FROM node_tags
   ...> WHERE key = 'shop'
   ...> GROUP BY value
   ...> ORDER by num DESC
   ...> LIMIT 15;
    
supermarket|210
convenience|119
department_store|38
bakery|34
bicycle|29
yes|23
clothes|20
confectionery|16
electronics|14
butcher|12
hairdresser|11
pet|10
jewelry|9
toys|9
variety_store|9
```

This top 15 types of shops in Singapore is clearly incomplete. For a nation of over 5.3 million, there has to be more than 12 butchers and 34 bakeries serving the consumers.

### Types and number of keys with 'school' as value

```sql
SELECT key, count(*) as num
FROM node_tags
WHERE value = 'school'
GROUP BY key
ORDER BY num DESC
;

amenity|21
building|1
```

Again, data here is incomplete, there are definitely more than 21 schools in Singapore.

### Number of restaurants in Singapore

```sql
sqlite> SELECT value, count(*)
   ...> FROM node_tags
   ...> WHERE value = 'restaurant';
   
restaurant|881
```

### Top 15 cuisines in Singapore

```sql
sqlite> SELECT node_tags.value, count(*) as num
   ...> FROM node_tags
   ...> JOIN (SELECT DISTINCT(id) FROM node_tags WHERE value = 'restaurant') a
   ...> ON node_tags.id = a.id
   ...> WHERE node_tags.key = 'cuisine'
   ...> GROUP BY node_tags.value
   ...> ORDER BY num DESC 
   ...> LIMIT 15;

chinese|89
japanese|40
korean|30
italian|29
indian|23
pizza|23
asian|20
french|14
thai|11
international|9
regional|7
burger|5
seafood|5
indonesian|4
vegetarian|4
```

Relatively, the portion of cuisines seems to be correct, just over 10% of restaurants (89 out of 881) are Chinese cuisines. However, absolutely, the numbers seem a little too low, there should be more restaurants in Singapore.

## Additional Ideas

It seems that there is no standardization of the entries in this dataset, for example in the school and restaurant data. The actual information about the school is probably in the dataset, but not labeled as "school". One possible way to quickly improve and clean the state of the Singapore data is perhaps to conduct online geographical-based competition or hackathon. 

Similar to Kaggle, participants can clean and update the dataset and be ranked by peers based on the quality and quantity of cleaning conducted. Rewards for participating can be "bragging rights" or acknowledgements in sociall media and relevant experience to be included in resumes for individuals looking to enter the data science industry.

Nevertheless, holding such competition may be challenging in terms of manpower and monetary resources for OpenStreetMap as it is a non-profit organization, ran by volunteers.

## Conclusion

This Singapore dataset is clearly incomplete and is still very "dirty". The cleaning conducted in this exercise is merely the tip of the iceberg. A significant portion of the cleaning will be non-programmatic since a larger proportion of the data was created by a larger number of users, possibly using manual entries, hence standardization of data becomes a significant factor in the quality of data.
