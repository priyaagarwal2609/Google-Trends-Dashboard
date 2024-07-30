# Google Trends Dashboard

This Power BI dashboard showcases data for trending Google keywords, sourced from Google APIs and integrated into Power BI tables. It provides a comprehensive view of keyword trends over time and across different regions. This structure ensures a detailed and interactive exploration of Google Trends data, offering valuable insights into search behavior and keyword popularity.

#### Keywords:
The dashboard focuses on 5 trending keywords:

Data Analyst
Web Developer
Software Developer
The Developer
India Jobs

#### Data Source:
The data in this dashboard is sourced from four Google APIs:

Country: Provides regional data.

Past 7 Days Searches: Captures search trends from the past week.

Date: Organizes data chronologically.

Keywords (Top and Rising): Highlights the most popular and emerging keywords.

#### Dashboard Pages:
The dashboard is divided into four main pages:

Overview: A summary of key metrics and visualizations.

![image](https://github.com/user-attachments/assets/181965ef-6ac6-422d-8a82-671ff675478a)

![image](https://github.com/user-attachments/assets/e53dd0b7-39f3-4fc1-a070-b7372033b5ce)

![image](https://github.com/user-attachments/assets/96935c82-48c4-4d86-a9f7-e864780d5f9d)

Keywords by Date: Tracks keyword performance over specific dates.

![image](https://github.com/user-attachments/assets/4e0c3c47-092b-4a11-9596-d3afdc52faae)

Rising & Top Keywords: Displays the most popular and emerging keywords.

![image](https://github.com/user-attachments/assets/88ed1d36-aac9-43dd-a66c-4213fe0cead4)

Real-Time Data: Provides up-to-the-minute insights on search trends.

![image](https://github.com/user-attachments/assets/f59ad431-f1f9-422f-b395-3a8f9328fee1)

#### Power Query Editor View

![image](https://github.com/user-attachments/assets/958b64a3-c9c2-44e7-b73b-d53529d4d7d9)


#### Query Editor Parameters

To facilitate easy updates and adjustments in both Power BI service and Power BI desktop, three parameters have been created:

Key: API key for authentication.

Country: Specifies the country for which data is being retrieved.

Keywords: Defines the keywords to be tracked.


#### Google Trend APIs M Code:

#### 1) Base_API_Country:

    let
    // Define the API endpoint
    apiUrl = "https://serpapi.com/search.json",


    // Define the parameters
    queryParams = [ engine = "google_trends", q = "Data Analyst,The Developer,Web Developer,India Jobs,Software Developer" , data_type = "GEO_MAP", date = "today 5-y", tz="-330",api_key = Key],

    // Combine the endpoint and parameters
    fullUrl = apiUrl & "?" & Uri.BuildQueryString(queryParams),

    // Make the HTTP request
    response = Web.Contents(fullUrl),

     // Parse the JSON response
    jsonResponse = Json.Document(response),

    // Convert the response to a table
    dataTable = Table.FromRecords({jsonResponse}),

    // Extract the relevant data
    comparedBreakdownByRegion = dataTable{0}[compared_breakdown_by_region]
    in
    comparedBreakdownByRegion

#### 2) Base_last_7_days_search

    let
    // Define the base URL for the API call
    BaseUrl = "https://serpapi.com/search",

    // Define the query parameters with engine,terms, data type, date, and time zone
    QueryParams = [engine="google_trends",q="Data Analyst,The Developer,Web Developer,India Jobs,Software Developer", data_type = "TIMESERIES",date = "now 7-d",tz = "-330",api_key = Key],

    // Generate the full URL with query parameters
    UrlWithParams = BaseUrl & "?" & Text.Combine(List.Transform(Record.FieldNames(QueryParams), 
    each _ & "=" & Uri.EscapeDataString(Record.Field(QueryParams, _))), "&"),

    // Fetch data from the API
    JsonResponse = Json.Document(Web.Contents(UrlWithParams)),

    // Extract the "interest_over_time" part from the JSON responseInterestOverTime = JsonResponse[#"interest_over_time"],
    // Extract the "interest_over_time" part from the JSON response
    timeline_data = InterestOverTime[timeline_data]
    in
    timeline_data


#### 3) Base_last_7_days_search

    let
    // Define the base URL for the API call
    BaseUrl = "https://serpapi.com/search",

    // Define the query parameters with engine, terms, data type, date, and time zone
    QueryParams = [engine = "google_trends", q = "Data Analyst,The Developer,Web Developer,India Jobs,Software Developer",data_type = "TIMESERIES", date = "all", tz = "-330",  api_key = Key],

    // Generate the full URL with query parameters
    UrlWithParams = BaseUrl & "?" & Text.Combine(List.Transform(Record.FieldNames(QueryParams), 
    each _ & "=" & Uri.EscapeDataString(Record.Field(QueryParams, _))), "&"),

    // Fetch data from the API
    JsonResponse = Json.Document(Web.Contents(UrlWithParams)),

    // Extract the "interest_over_time" part from the JSON response
    InterestOverTime = JsonResponse[#"interest_over_time"],
    // Extract the "interest_over_time" part from the JSON response
    timeline_data = InterestOverTime[timeline_data]
    in
    timeline_data

#### 4) Base_related_keywords

    let
    // Define the API endpoint
    apiUrl = "https://serpapi.com/search.json",

    // Define the parameters
    queryParams = [engine = "google_trends",q = Related_Keyword, data_type = "RELATED_TOPICS",api_key = Key],

    // Combine the endpoint and parameters to create the full URL
    fullUrl = apiUrl & "?" & Uri.BuildQueryString(queryParams),

    // Make the HTTP request and get the response
    response = Web.Contents(fullUrl),

    // Parse the JSON response
    jsonResponse = Json.Document(response),
    // Parse the JSON response
    related_topics = jsonResponse[related_topics]
    in
    related_topics













