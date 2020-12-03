# Outlet Recommendation using Power BI
Recommending new outlet based on orders placed before

## About
We can gain a lot of insights with historical data like order history. In this project I am writing a glimpse of how I used the data on deliveries made by a restaurant to its customer and further suggested location to open new outlet so that they can optimize their delivery time within 20 minutes.<br\>
For this project I used Microsoft Power Bi as a visualization and analysis tool. The reason I used Power Bi is that it has good easy visualization of map, it has built-in features to manage radius or drive-time and it can translate the address to geo-location which makes task relatively easy.
If you have Powebi desktop you can download file <a href="/recommend_outlet.pbix" target="_blank"> here </a>and follow along.

## Data, Query and Visualization
We will be working 570 orders and each order will have their longitude and latitude.Data will be in my github  <a href="/data/outlet_analysis.csv" target="_blank">here. </a>
Querying in power bi is simple we will just load the file, Incase you need DAX code:

```javascript
let
    Source = Csv.Document(Web.Contents("https://raw.githubusercontent.com/roesta07/Outlet-Recommendation/main/data/outlet_analysis.csv"),[Delimiter=",", Columns=5, Encoding=1252,     QuoteStyle=QuoteStyle.None]),
    #"Promoted Headers" = Table.PromoteHeaders(Source, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"order_no", Int64.Type}, {"district", type text}, {"address", type text}, {"latitude", type number}, {"longitude", type number}})
in
    #"Changed Type"in

```
Now let’s plot the data using ArcGIS maps for power bi

<img src="/img/capture1.PNG" alt="delivery distrubution" width="800" height="800">

Now we are averaging the data using simple rounding method also i will be rounding to one digit to make it more simple. Incase you need the DAX code:
```javascript
    Source = Csv.Document(File.Contents("C:\Dropbox\Dropbox\prj\data_projects\outlet location\outlet_analysis.csv"),[Delimiter=",", Columns=5, Encoding=1252,                         QuoteStyle=QuoteStyle.None]),
    #"Promoted Headers" = Table.PromoteHeaders(Source, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"order_no", Int64.Type}, {"district", type text}, {"address", type text}, {"latitude", type number},           {"longitude", type number}}),
    #"Rounded Off" = Table.TransformColumns(#"Changed Type",{{"latitude", each Number.Round(_, 1), type number}}),
    #"Rounded Off1" = Table.TransformColumns(#"Rounded Off",{{"longitude", each Number.Round(_, 1), type number}}),
    #"Removed Duplicates" = Table.Distinct(#"Rounded Off1", {"latitude", "longitude"})
in
    #"Removed Duplicates"
```
Let’s plot the data and assign the drive time of 20 minutes.This feature will be in the map tools (left-top corner):

<img src="/img/capture2.PNG" alt="delivery distrubution" width="800" height="800">


## Findings and Suggestion
-	We can see all the probable locations for our new outlet colored in purple. We can see the big patches in the middle, it can be because of the frequency of the orders or could have accessible roads.
-	 There are more than three patches of purple or more than 3 suggestions for our new outlook; but I would suggest the restaurant to open only two outlets where the purple color overlap and gets darker.
-	Although we cannot ignore other spot suggested as they can be the next potential but further analysis has to be done to answer questions related to cost, accessibility to road etc.






