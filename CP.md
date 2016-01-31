# Machine learning final project
Antonio Mart?nez Pineda  
30 de enero de 2016  



#Summary#

A machine learning was developed for predict the performance of six men doing a set of 10 dumbell in five different ways. The model for machine learning was developed with randomforest and crossvalidated with the caret package in R.
Accuracy for the model in the training data was .999 and prediction's results was 20/20.

#Introduction#

The goal of this project was to create a machine learning to predict the way in dumbell excercise was made by six men performing one set of ten repetitions according to the specification (Class A), throwing elbows to the front (Class B), lifting the dumbbell only halfway (Class C), lowering the dumbbell only halfway (Class D), and throwing the hips to the front (Class E).

#Data obtaning and exploratory analysis#

The [training](https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv) and [testing data](https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv) sets was downloading from the respective links and a exploratory analysis for missing data was made.

In this report we assume the data sets are in the work directory.


```r
# loading data
library(data.table)
# if('pml-training.csv'%in%row.names(list.files())==F){
# url<-'https://d396qusza40orc.cloudfront.net/predmachlearn/pml-t#raining.csv'
# download.file(url,destfile = 'pml-training.csv') }
training <- fread("pml-training.csv", verbose = T, na.strings = c("NA", "N/A", 
    "#DIV/0!"), stringsAsFactors = T)
```

```
Input contains no \n. Taking this to be a filename to open
File opened, filesize is 0.009989 GB.
Memory mapping ... ok
Detected eol as \r\n (CRLF) in that order, the Windows standard.
Positioned on line 1 after skip or autostart
This line is the autostart and not blank so searching up for the last non-blank ... line 1
Detecting sep ... ','
Detected 160 columns. Longest stretch was from line 1 to line 30
Starting data input on line 1 (either column names or first row of data). First 10 characters: ,user_name
All the fields on line 1 are character fields. Treating as the column names.
Count of eol: 19623 (including 1 at the end)
Count of sep: 3119898
nrow = MIN( nsep [3119898] / ncol [160] -1, neol [19623] - nblank [1] ) = 19622
Type codes (   first 5 rows): 1411441333100000000000000000000000003331111111311000000000033311111100000000000000033300000000000000010000000000133111111331000000000000000100000000003331111114
Type codes (+ middle 5 rows): 1411441333100000000000000000000000003331111111331000000000033311111100000000000000033300000000000000010000000000333111111333000000000000000100000000003331111114
Type codes (+   last 5 rows): 1411441333100000000000000000000000003331111113331000000000033311111100000000000000033300000000000000010000000000333111111333000000000000000100000000003331111114
Type codes: 1411441333100000000000000000000000003331111113331000000000033311111100000000000000033300000000000000010000000000333111111333000000000000000100000000003331111114 (after applying colClasses and integer64)
Type codes: 1411441333100000000000000000000000003331111113331000000000033311111100000000000000033300000000000000010000000000333111111333000000000000000100000000003331111114 (after applying drop or select (if supplied)
Allocating 160 column slots (160 - 0 dropped)
Bumping column 12 from LGL to INT on data row 24, field contains '5.587755'
Bumping column 12 from INT to INT64 on data row 24, field contains '5.587755'
Bumping column 12 from INT64 to REAL on data row 24, field contains '5.587755'
Bumping column 15 from LGL to INT on data row 24, field contains '2.713152'
Bumping column 15 from INT to INT64 on data row 24, field contains '2.713152'
Bumping column 15 from INT64 to REAL on data row 24, field contains '2.713152'
Bumping column 18 from LGL to INT on data row 24, field contains '-94.3'
Bumping column 18 from INT to INT64 on data row 24, field contains '-94.3'
Bumping column 18 from INT64 to REAL on data row 24, field contains '-94.3'
Bumping column 19 from LGL to INT on data row 24, field contains '3'
Bumping column 20 from LGL to INT on data row 24, field contains '5.6'
Bumping column 20 from INT to INT64 on data row 24, field contains '5.6'
Bumping column 20 from INT64 to REAL on data row 24, field contains '5.6'
Bumping column 21 from LGL to INT on data row 24, field contains '-94.4'
Bumping column 21 from INT to INT64 on data row 24, field contains '-94.4'
Bumping column 21 from INT64 to REAL on data row 24, field contains '-94.4'
Bumping column 22 from LGL to INT on data row 24, field contains '3'
Bumping column 23 from LGL to INT on data row 24, field contains '5.6'
Bumping column 23 from INT to INT64 on data row 24, field contains '5.6'
Bumping column 23 from INT64 to REAL on data row 24, field contains '5.6'
Bumping column 24 from LGL to INT on data row 24, field contains '0.1'
Bumping column 24 from INT to INT64 on data row 24, field contains '0.1'
Bumping column 24 from INT64 to REAL on data row 24, field contains '0.1'
Bumping column 25 from LGL to INT on data row 24, field contains '0'
Bumping column 26 from LGL to INT on data row 24, field contains '0'
Bumping column 27 from LGL to INT on data row 24, field contains '0'
Bumping column 28 from LGL to INT on data row 24, field contains '1.5'
Bumping column 28 from INT to INT64 on data row 24, field contains '1.5'
Bumping column 28 from INT64 to REAL on data row 24, field contains '1.5'
Bumping column 29 from LGL to INT on data row 24, field contains '0.1'
Bumping column 29 from INT to INT64 on data row 24, field contains '0.1'
Bumping column 29 from INT64 to REAL on data row 24, field contains '0.1'
Bumping column 30 from LGL to INT on data row 24, field contains '0'
Bumping column 31 from LGL to INT on data row 24, field contains '8.1'
Bumping column 31 from INT to INT64 on data row 24, field contains '8.1'
Bumping column 31 from INT64 to REAL on data row 24, field contains '8.1'
Bumping column 32 from LGL to INT on data row 24, field contains '0'
Bumping column 33 from LGL to INT on data row 24, field contains '0'
Bumping column 34 from LGL to INT on data row 24, field contains '-94.4'
Bumping column 34 from INT to INT64 on data row 24, field contains '-94.4'
Bumping column 34 from INT64 to REAL on data row 24, field contains '-94.4'
Bumping column 35 from LGL to INT on data row 24, field contains '0'
Bumping column 36 from LGL to INT on data row 24, field contains '0'
Bumping column 50 from LGL to INT on data row 24, field contains '0'
Bumping column 51 from LGL to INT on data row 24, field contains '-128.4898'
Bumping column 51 from INT to INT64 on data row 24, field contains '-128.4898'
Bumping column 51 from INT64 to REAL on data row 24, field contains '-128.4898'
Bumping column 52 from LGL to INT on data row 24, field contains '0.5051'
Bumping column 52 from INT to INT64 on data row 24, field contains '0.5051'
Bumping column 52 from INT64 to REAL on data row 24, field contains '0.5051'
Bumping column 53 from LGL to INT on data row 24, field contains '0.2551'
Bumping column 53 from INT to INT64 on data row 24, field contains '0.2551'
Bumping column 53 from INT64 to REAL on data row 24, field contains '0.2551'
Bumping column 54 from LGL to INT on data row 24, field contains '21.4327'
Bumping column 54 from INT to INT64 on data row 24, field contains '21.4327'
Bumping column 54 from INT64 to REAL on data row 24, field contains '21.4327'
Bumping column 55 from LGL to INT on data row 24, field contains '0.4836'
Bumping column 55 from INT to INT64 on data row 24, field contains '0.4836'
Bumping column 55 from INT64 to REAL on data row 24, field contains '0.4836'
Bumping column 56 from LGL to INT on data row 24, field contains '0.2339'
Bumping column 56 from INT to INT64 on data row 24, field contains '0.2339'
Bumping column 56 from INT64 to REAL on data row 24, field contains '0.2339'
Bumping column 57 from LGL to INT on data row 24, field contains '-161'
Bumping column 58 from LGL to INT on data row 24, field contains '0'
Bumping column 59 from LGL to INT on data row 24, field contains '0'
Bumping column 69 from LGL to INT on data row 24, field contains '-1.05825'
Bumping column 69 from INT to INT64 on data row 24, field contains '-1.05825'
Bumping column 69 from INT64 to REAL on data row 24, field contains '-1.05825'
Bumping column 72 from LGL to INT on data row 24, field contains '0.13832'
Bumping column 72 from INT to INT64 on data row 24, field contains '0.13832'
Bumping column 72 from INT64 to REAL on data row 24, field contains '0.13832'
Bumping column 75 from LGL to INT on data row 24, field contains '22.3'
Bumping column 75 from INT to INT64 on data row 24, field contains '22.3'
Bumping column 75 from INT64 to REAL on data row 24, field contains '22.3'
Bumping column 76 from LGL to INT on data row 24, field contains '-161'
Bumping column 77 from LGL to INT on data row 24, field contains '34'
Bumping column 78 from LGL to INT on data row 24, field contains '20.7'
Bumping column 78 from INT to INT64 on data row 24, field contains '20.7'
Bumping column 78 from INT64 to REAL on data row 24, field contains '20.7'
Bumping column 79 from LGL to INT on data row 24, field contains '-161'
Bumping column 80 from LGL to INT on data row 24, field contains '34'
Bumping column 81 from LGL to INT on data row 24, field contains '1.6'
Bumping column 81 from INT to INT64 on data row 24, field contains '1.6'
Bumping column 81 from INT64 to REAL on data row 24, field contains '1.6'
Bumping column 82 from LGL to INT on data row 24, field contains '0'
Bumping column 83 from LGL to INT on data row 24, field contains '0'
Bumping column 87 from LGL to INT on data row 24, field contains '-0.6209'
Bumping column 87 from INT to INT64 on data row 24, field contains '-0.6209'
Bumping column 87 from INT64 to REAL on data row 24, field contains '-0.6209'
Bumping column 88 from LGL to INT on data row 24, field contains '-0.6149'
Bumping column 88 from INT to INT64 on data row 24, field contains '-0.6149'
Bumping column 88 from INT64 to REAL on data row 24, field contains '-0.6149'
Bumping column 90 from LGL to INT on data row 24, field contains '-0.096'
Bumping column 90 from INT to INT64 on data row 24, field contains '-0.096'
Bumping column 90 from INT64 to REAL on data row 24, field contains '-0.096'
Bumping column 91 from LGL to INT on data row 24, field contains '0.1049'
Bumping column 91 from INT to INT64 on data row 24, field contains '0.1049'
Bumping column 91 from INT64 to REAL on data row 24, field contains '0.1049'
Bumping column 93 from LGL to INT on data row 24, field contains '-70.1'
Bumping column 93 from INT to INT64 on data row 24, field contains '-70.1'
Bumping column 93 from INT64 to REAL on data row 24, field contains '-70.1'
Bumping column 94 from LGL to INT on data row 24, field contains '-84.3'
Bumping column 94 from INT to INT64 on data row 24, field contains '-84.3'
Bumping column 94 from INT64 to REAL on data row 24, field contains '-84.3'
Bumping column 95 from LGL to INT on data row 24, field contains '-0.6'
Bumping column 95 from INT to INT64 on data row 24, field contains '-0.6'
Bumping column 95 from INT64 to REAL on data row 24, field contains '-0.6'
Bumping column 96 from LGL to INT on data row 24, field contains '-71'
Bumping column 97 from LGL to INT on data row 24, field contains '-85.3'
Bumping column 97 from INT to INT64 on data row 24, field contains '-85.3'
Bumping column 97 from INT64 to REAL on data row 24, field contains '-85.3'
Bumping column 98 from LGL to INT on data row 24, field contains '-0.6'
Bumping column 98 from INT to INT64 on data row 24, field contains '-0.6'
Bumping column 98 from INT64 to REAL on data row 24, field contains '-0.6'
Bumping column 99 from LGL to INT on data row 24, field contains '0.96'
Bumping column 99 from INT to INT64 on data row 24, field contains '0.96'
Bumping column 99 from INT64 to REAL on data row 24, field contains '0.96'
Bumping column 100 from LGL to INT on data row 24, field contains '1.02'
Bumping column 100 from INT to INT64 on data row 24, field contains '1.02'
Bumping column 100 from INT64 to REAL on data row 24, field contains '1.02'
Bumping column 101 from LGL to INT on data row 24, field contains '0'
Bumping column 103 from LGL to INT on data row 24, field contains '0.0204'
Bumping column 103 from INT to INT64 on data row 24, field contains '0.0204'
Bumping column 103 from INT64 to REAL on data row 24, field contains '0.0204'
Bumping column 104 from LGL to INT on data row 24, field contains '13.1942'
Bumping column 104 from INT to INT64 on data row 24, field contains '13.1942'
Bumping column 104 from INT64 to REAL on data row 24, field contains '13.1942'
Bumping column 105 from LGL to INT on data row 24, field contains '0.1811'
Bumping column 105 from INT to INT64 on data row 24, field contains '0.1811'
Bumping column 105 from INT64 to REAL on data row 24, field contains '0.1811'
Bumping column 106 from LGL to INT on data row 24, field contains '0.0328'
Bumping column 106 from INT to INT64 on data row 24, field contains '0.0328'
Bumping column 106 from INT64 to REAL on data row 24, field contains '0.0328'
Bumping column 107 from LGL to INT on data row 24, field contains '-70.5253'
Bumping column 107 from INT to INT64 on data row 24, field contains '-70.5253'
Bumping column 107 from INT64 to REAL on data row 24, field contains '-70.5253'
Bumping column 108 from LGL to INT on data row 24, field contains '0.2384'
Bumping column 108 from INT to INT64 on data row 24, field contains '0.2384'
Bumping column 108 from INT64 to REAL on data row 24, field contains '0.2384'
Bumping column 109 from LGL to INT on data row 24, field contains '0.0568'
Bumping column 109 from INT to INT64 on data row 24, field contains '0.0568'
Bumping column 109 from INT64 to REAL on data row 24, field contains '0.0568'
Bumping column 110 from LGL to INT on data row 24, field contains '-84.8053'
Bumping column 110 from INT to INT64 on data row 24, field contains '-84.8053'
Bumping column 110 from INT64 to REAL on data row 24, field contains '-84.8053'
Bumping column 111 from LGL to INT on data row 24, field contains '0.256'
Bumping column 111 from INT to INT64 on data row 24, field contains '0.256'
Bumping column 111 from INT64 to REAL on data row 24, field contains '0.256'
Bumping column 112 from LGL to INT on data row 24, field contains '0.0656'
Bumping column 112 from INT to INT64 on data row 24, field contains '0.0656'
Bumping column 112 from INT64 to REAL on data row 24, field contains '0.0656'
Bumping column 125 from LGL to INT on data row 24, field contains '-0.368'
Bumping column 125 from INT to INT64 on data row 24, field contains '-0.368'
Bumping column 125 from INT64 to REAL on data row 24, field contains '-0.368'
Bumping column 126 from LGL to INT on data row 24, field contains '-2.0402'
Bumping column 126 from INT to INT64 on data row 24, field contains '-2.0402'
Bumping column 126 from INT64 to REAL on data row 24, field contains '-2.0402'
Bumping column 128 from LGL to INT on data row 24, field contains '0.2113'
Bumping column 128 from INT to INT64 on data row 24, field contains '0.2113'
Bumping column 128 from INT64 to REAL on data row 24, field contains '0.2113'
Bumping column 129 from LGL to INT on data row 24, field contains '-0.2117'
Bumping column 129 from INT to INT64 on data row 24, field contains '-0.2117'
Bumping column 129 from INT64 to REAL on data row 24, field contains '-0.2117'
Bumping column 131 from LGL to INT on data row 24, field contains '-63.7'
Bumping column 131 from INT to INT64 on data row 24, field contains '-63.7'
Bumping column 131 from INT64 to REAL on data row 24, field contains '-63.7'
Bumping column 132 from LGL to INT on data row 24, field contains '-151'
Bumping column 133 from LGL to INT on data row 24, field contains '-0.4'
Bumping column 133 from INT to INT64 on data row 24, field contains '-0.4'
Bumping column 133 from INT64 to REAL on data row 24, field contains '-0.4'
Bumping column 134 from LGL to INT on data row 24, field contains '-64'
Bumping column 135 from LGL to INT on data row 24, field contains '-152'
Bumping column 136 from LGL to INT on data row 24, field contains '-0.4'
Bumping column 136 from INT to INT64 on data row 24, field contains '-0.4'
Bumping column 136 from INT64 to REAL on data row 24, field contains '-0.4'
Bumping column 137 from LGL to INT on data row 24, field contains '0.3'
Bumping column 137 from INT to INT64 on data row 24, field contains '0.3'
Bumping column 137 from INT64 to REAL on data row 24, field contains '0.3'
Bumping column 138 from LGL to INT on data row 24, field contains '1'
Bumping column 139 from LGL to INT on data row 24, field contains '0'
Bumping column 141 from LGL to INT on data row 24, field contains '0'
Bumping column 142 from LGL to INT on data row 24, field contains '27.40204'
Bumping column 142 from INT to INT64 on data row 24, field contains '27.40204'
Bumping column 142 from INT64 to REAL on data row 24, field contains '27.40204'
Bumping column 143 from LGL to INT on data row 24, field contains '0.45893'
Bumping column 143 from INT to INT64 on data row 24, field contains '0.45893'
Bumping column 143 from INT64 to REAL on data row 24, field contains '0.45893'
Bumping column 144 from LGL to INT on data row 24, field contains '0.21062'
Bumping column 144 from INT to INT64 on data row 24, field contains '0.21062'
Bumping column 144 from INT64 to REAL on data row 24, field contains '0.21062'
Bumping column 145 from LGL to INT on data row 24, field contains '-63.89388'
Bumping column 145 from INT to INT64 on data row 24, field contains '-63.89388'
Bumping column 145 from INT64 to REAL on data row 24, field contains '-63.89388'
Bumping column 146 from LGL to INT on data row 24, field contains '0.07474'
Bumping column 146 from INT to INT64 on data row 24, field contains '0.07474'
Bumping column 146 from INT64 to REAL on data row 24, field contains '0.07474'
Bumping column 147 from LGL to INT on data row 24, field contains '0.00559'
Bumping column 147 from INT to INT64 on data row 24, field contains '0.00559'
Bumping column 147 from INT64 to REAL on data row 24, field contains '0.00559'
Bumping column 148 from LGL to INT on data row 24, field contains '-151.44898'
Bumping column 148 from INT to INT64 on data row 24, field contains '-151.44898'
Bumping column 148 from INT64 to REAL on data row 24, field contains '-151.44898'
Bumping column 149 from LGL to INT on data row 24, field contains '0.50254'
Bumping column 149 from INT to INT64 on data row 24, field contains '0.50254'
Bumping column 149 from INT64 to REAL on data row 24, field contains '0.50254'
Bumping column 150 from LGL to INT on data row 24, field contains '0.25255'
Bumping column 150 from INT to INT64 on data row 24, field contains '0.25255'
Bumping column 150 from INT64 to REAL on data row 24, field contains '0.25255'
Bumping column 32 from INT to INT64 on data row 52, field contains '0.2'
Bumping column 32 from INT64 to REAL on data row 52, field contains '0.2'
Bumping column 35 from INT to INT64 on data row 52, field contains '0.1'
Bumping column 35 from INT64 to REAL on data row 52, field contains '0.1'
Bumping column 36 from INT to INT64 on data row 52, field contains '0.01'
Bumping column 36 from INT64 to REAL on data row 52, field contains '0.01'
Bumping column 57 from INT to INT64 on data row 52, field contains '-161.5882'
Bumping column 57 from INT64 to REAL on data row 52, field contains '-161.5882'
Bumping column 58 from INT to INT64 on data row 52, field contains '0.4971'
Bumping column 58 from INT64 to REAL on data row 52, field contains '0.4971'
Bumping column 59 from INT to INT64 on data row 52, field contains '0.2471'
Bumping column 59 from INT64 to REAL on data row 52, field contains '0.2471'
Bumping column 70 from LGL to INT on data row 52, field contains '-1.94121'
Bumping column 70 from INT to INT64 on data row 52, field contains '-1.94121'
Bumping column 70 from INT64 to REAL on data row 52, field contains '-1.94121'
Bumping column 73 from LGL to INT on data row 52, field contains '0.36953'
Bumping column 73 from INT to INT64 on data row 52, field contains '0.36953'
Bumping column 73 from INT64 to REAL on data row 52, field contains '0.36953'
Bumping column 96 from INT to INT64 on data row 52, field contains '-71.4'
Bumping column 96 from INT64 to REAL on data row 52, field contains '-71.4'
Bumping column 134 from INT to INT64 on data row 52, field contains '-63.9'
Bumping column 134 from INT64 to REAL on data row 52, field contains '-63.9'
Bumping column 13 from LGL to INT on data row 165, field contains '-1.29859'
Bumping column 13 from INT to INT64 on data row 165, field contains '-1.29859'
Bumping column 13 from INT64 to REAL on data row 165, field contains '-1.29859'
Bumping column 16 from LGL to INT on data row 165, field contains '0.88139'
Bumping column 16 from INT to INT64 on data row 165, field contains '0.88139'
Bumping column 16 from INT64 to REAL on data row 165, field contains '0.88139'
Bumping column 27 from INT to INT64 on data row 165, field contains '0.2'
Bumping column 27 from INT64 to REAL on data row 165, field contains '0.2'
Bumping column 50 from INT to INT64 on data row 165, field contains '0.0278'
Bumping column 50 from INT64 to REAL on data row 165, field contains '0.0278'
Bumping column 71 from LGL to INT on data row 165, field contains '36'
Bumping column 74 from LGL to INT on data row 165, field contains '-6'
Bumping column 30 from INT to INT64 on data row 210, field contains '0.1'
Bumping column 30 from INT64 to REAL on data row 210, field contains '0.1'
Bumping column 71 from INT to INT64 on data row 210, field contains '19.81071'
Bumping column 71 from INT64 to REAL on data row 210, field contains '19.81071'
Bumping column 74 from INT to INT64 on data row 210, field contains '-4.57508'
Bumping column 74 from INT64 to REAL on data row 210, field contains '-4.57508'
Bumping column 141 from INT to INT64 on data row 210, field contains '0.26768'
Bumping column 141 from INT64 to REAL on data row 210, field contains '0.26768'
Bumping column 76 from INT to INT64 on data row 241, field contains '8.8'
Bumping column 76 from INT64 to REAL on data row 241, field contains '8.8'
Bumping column 79 from INT to INT64 on data row 241, field contains '-63.3'
Bumping column 79 from INT64 to REAL on data row 241, field contains '-63.3'
Bumping column 82 from INT to INT64 on data row 241, field contains '72.13'
Bumping column 82 from INT64 to REAL on data row 241, field contains '72.13'
Bumping column 33 from INT to INT64 on data row 314, field contains '0.1'
Bumping column 33 from INT64 to REAL on data row 314, field contains '0.1'
Bumping column 132 from INT to INT64 on data row 532, field contains '99.4'
Bumping column 132 from INT64 to REAL on data row 532, field contains '99.4'
Bumping column 135 from INT to INT64 on data row 532, field contains '74.1'
Bumping column 135 from INT64 to REAL on data row 532, field contains '74.1'
Bumping column 138 from INT to INT64 on data row 532, field contains '25.3'
Bumping column 138 from INT64 to REAL on data row 532, field contains '25.3'
Bumping column 121 from INT to INT64 on data row 5373, field contains '69.6'
Bumping column 121 from INT64 to REAL on data row 5373, field contains '69.6'
Bumping column 158 from INT to INT64 on data row 5373, field contains '-0.123'
Bumping column 158 from INT64 to REAL on data row 5373, field contains '-0.123'
Bumping column 159 from INT to INT64 on data row 5373, field contains '-0.0917'
Bumping column 159 from INT64 to REAL on data row 5373, field contains '-0.0917'

Read 51.0% of 19622 rows
Read 19622 rows and 160 (of 160) columns from 0.010 GB file in 00:00:04
Read 19622 rows. Exactly what was estimated and allocated up front
   0.063s (  2%) Memory map (rerun may be quicker)
   0.005s (  0%) sep and header detection
   0.787s ( 21%) Count rows (wc -l)
   0.003s (  0%) Column type detection (first, middle and last 5 rows)
   0.265s (  7%) Allocation of 19622x160 result (xMB) in RAM
   2.505s ( 66%) Reading data
   0.173s (  5%) Allocation for type bumps (if any), including gc time if triggered
   0.012s (  0%) Coercing data already read in type bumps (if any)
   0.002s (  0%) Changing na.strings to NA
   3.815s        Total
Converting column(s) [user_name, cvtd_timestamp, new_window, classe] from 'char' to 'factor'
```

```r
# if('pml-testing.csv'%in%row.names(list.files())==F){
# url<-'https://d396qusza40orc.cloudfront.net/predmachlearn/pml-t#esting.csv'
# download.file(url,destfile = 'pml-testing.csv') }
testing <- fread("pml-testing.csv", verbose = T, na.strings = c("NA", "N/A", 
    "#DIV/0!"), stringsAsFactors = T)
```

```
Input contains no \n. Taking this to be a filename to open
File opened, filesize is 0.000012 GB.
Memory mapping ... ok
Detected eol as \r\n (CRLF) in that order, the Windows standard.
Positioned on line 1 after skip or autostart
This line is the autostart and not blank so searching up for the last non-blank ... line 1
Detecting sep ... ','
Detected 160 columns. Longest stretch was from line 1 to line 21
Starting data input on line 1 (either column names or first row of data). First 10 characters: ,user_name
All the fields on line 1 are character fields. Treating as the column names.
Count of eol: 21 (including 1 at the end)
Count of sep: 3180
nrow = MIN( nsep [3180] / ncol [160] -1, neol [21] - nblank [1] ) = 20
Type codes (   first 5 rows): 1411441333100000000000000000000000003331111113311000000000033311111100000000000000033300000000000000010000000000333111111133000000000000000100000000003331111111
Type codes (+ middle 5 rows): 1411441333100000000000000000000000003331111113331000000000033311111100000000000000033300000000000000010000000000333111111333000000000000000100000000003331111111
Type codes (+   last 5 rows): 1411441333100000000000000000000000003331111113331000000000033311111100000000000000033300000000000000010000000000333111111333000000000000000100000000003331111111
Type codes: 1411441333100000000000000000000000003331111113331000000000033311111100000000000000033300000000000000010000000000333111111333000000000000000100000000003331111111 (after applying colClasses and integer64)
Type codes: 1411441333100000000000000000000000003331111113331000000000033311111100000000000000033300000000000000010000000000333111111333000000000000000100000000003331111111 (after applying drop or select (if supplied)
Allocating 160 column slots (160 - 0 dropped)
Read 20 rows. Exactly what was estimated and allocated up front
   0.014s ( 70%) Memory map (rerun may be quicker)
   0.001s (  5%) sep and header detection
   0.000s (  0%) Count rows (wc -l)
   0.003s ( 15%) Column type detection (first, middle and last 5 rows)
   0.000s (  0%) Allocation of 20x160 result (xMB) in RAM
   0.002s ( 10%) Reading data
   0.000s (  0%) Allocation for type bumps (if any), including gc time if triggered
   0.000s (  0%) Coercing data already read in type bumps (if any)
   0.000s (  0%) Changing na.strings to NA
   0.020s        Total
Converting column(s) [user_name, cvtd_timestamp, new_window] from 'char' to 'factor'
```

```r
na.sums <- apply(training, 2, function(x) sum(is.na(x)))
del <- names(na.sums[na.sums > 1900])
# deleting variables with more than 19000 NA's
training <- training[, `:=`(c("V1", del), NULL)]
dim(training)
```

```
[1] 19622    59
```

```r
na.sums <- apply(testing, 2, function(x) sum(is.na(x)))
del <- names(na.sums[na.sums > 19])
testing <- testing[, `:=`(c("V1", del), NULL)]
dim(testing)
```

```
[1] 20 59
```

Only variables with less than 19000 missing data was taking into account for the following analysis.

#Analysis#

First try for machine learning was to use all available variables in a random forest model. This achieved the minimum accuracy required in the project's instructions. the machine learning model was carried out with the `caret`package and requires to be installed the packages `randomForest`and `e1071`. The use of parallel packages was recomended in with the available CPU -sadly my laptop has only  2 :(.

Crossvalidated method was carried out in the model as default.


```r
library(caret)
library(parallel)
set.seed(1235)
c <- makeCluster(2)
model1 <- train(classe ~ ., method = "rf", data = training, trControl = trainControl(method = "cv"), 
    number = 3)
stopCluster(c)
model1
```

```
Random Forest 

19622 samples
   58 predictor
    5 classes: 'A', 'B', 'C', 'D', 'E' 

No pre-processing
Resampling: Cross-Validated (10 fold) 
Summary of sample sizes: 17660, 17660, 17660, 17660, 17659, 17658, ... 
Resampling results across tuning parameters:

  mtry  Accuracy   Kappa      Accuracy SD   Kappa SD    
   2    0.9915916  0.9893630  0.0027916251  0.0035318319
  41    0.9995416  0.9994202  0.0007759216  0.0009814373
  80    0.9991338  0.9989044  0.0007220635  0.0009133570

Accuracy was used to select the optimal model using  the largest value.
The final value used for the model was mtry = 41. 
```

```r
model1$finalModel
```

```

Call:
 randomForest(x = x, y = y, mtry = param$mtry, number = 3) 
               Type of random forest: classification
                     Number of trees: 500
No. of variables tried at each split: 41

        OOB estimate of  error rate: 0.04%
Confusion matrix:
     A    B    C    D    E  class.error
A 5580    0    0    0    0 0.0000000000
B    1 3795    1    0    0 0.0005267316
C    0    3 3419    0    0 0.0008766803
D    0    0    2 3213    1 0.0009328358
E    0    0    0    0 3607 0.0000000000
```

Once model was perfromance a prediction for the 20 samples in testing set was made.


```r
pred <- predict(model1, newdata = testing)
table(pred)
```

```
pred
A B C D E 
7 8 1 1 3 
```

The results of predictions was 20/20 in the submit quiz.
