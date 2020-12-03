# Outlet Recommendation using Power BI
Recommending new outlet based on orders placed before

## About
We can gain a lot of insights with historical data like order history. In this project I am writing a glimpse of how I used the data on deliveries made by a restaurant to its customer and further suggested location to open new outlet so that they can optimize their delivery time within 20 minutes.<br\>
For this project I used Microsoft Power Bi as a visualization and analysis tool. The reason I used Power Bi is that it has good easy visualization of map, it has built-in features to manage radius or drive-time and it can translate the address to geo-location which makes task relatively easy.
If you have Powebi desktop you can download file <a href="/recommend_outlet.pbix" target="_blank"> here </a>and follow along.

## Data Query and visualization
We will be working 570 orders and each order will have their longitude and latitude.Data will be in my github  <a href="/data/outlet_analysis.csv" target="_blank">here. </a>
Querying in power bi is simple we will just load the file, Incase you need DAX code

```javascript
let
    Source = Csv.Document(Web.Contents("https://raw.githubusercontent.com/roesta07/Outlet-Recommendation/main/data/outlet_analysis.csv"),[Delimiter=",", Columns=5, Encoding=1252,     QuoteStyle=QuoteStyle.None]),
    #"Promoted Headers" = Table.PromoteHeaders(Source, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"order_no", Int64.Type}, {"district", type text}, {"address", type text}, {"latitude", type number}, {"longitude", type number}})
in
    #"Changed Type"in

```


