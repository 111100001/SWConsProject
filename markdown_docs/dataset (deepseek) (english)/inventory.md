## FunctionDef update_prediction_data
**update_prediction_data**: The function of update_prediction_data is to fetch prediction data, process it, and write the filtered results to Cassandra.

**parameters**: The parameters of this Function.
· This function does not take any explicit parameters. It relies on predefined constants and helper functions within the module.

**Code Description**: The description of this Function.
The update_prediction_data function performs the following steps to fetch, process, and store prediction data:

1. It calls the helper function _min_daily_pageviews_by_sr with the constant NDAYS_TO_QUERY to retrieve a dictionary mapping subreddit names to their minimum daily pageviews over the specified number of days. This dictionary is stored in the variable min_daily_by_sr.

2. The function then checks if the dictionary contains an entry with an empty string key (''). This key represents the front page of the platform. If such an entry exists, the function combines its value with the value of the front page subreddit (DefaultSR.name.lower()) in the dictionary. The empty key is then removed from the dictionary to clean up the data.

3. The function filters the dictionary to retain only those subreddits whose minimum daily pageviews exceed 100. This is done using a dictionary comprehension that iterates over the items in min_daily_by_sr and includes only those entries where the pageview count is greater than 100. The filtered results are stored in the variable filtered.

4. Finally, the function writes the filtered dictionary to Cassandra using the PromoMetrics.set method. The data is stored under the key specified by the constant MIN_DAILY_CASS_KEY.

The function relies on the helper function _min_daily_pageviews_by_sr to fetch the initial data and on the PromoMetrics.set method to persist the processed data. It ensures that only relevant subreddits (those with significant traffic) are stored, optimizing storage and retrieval efficiency.

**Note**: Points to note about the use of the code
- The function assumes that the constants NDAYS_TO_QUERY and MIN_DAILY_CASS_KEY are defined elsewhere in the codebase.
- The function is designed to handle edge cases, such as the presence of an empty key representing the front page, ensuring data consistency.
- The filtering threshold (100 pageviews) is hardcoded and may need adjustment based on specific use cases or traffic patterns.
## FunctionDef _min_daily_pageviews_by_sr(ndays, end_date)
**_min_daily_pageviews_by_sr**: The function of _min_daily_pageviews_by_sr is to return a dictionary mapping subreddit names to their minimum daily pageviews over a specified number of days.

**parameters**: The parameters of this Function.
· ndays: The number of days to query for minimum pageviews. Defaults to NDAYS_TO_QUERY.
· end_date: The end date for the query range. If not provided, it defaults to one day before the last modified date of traffic data.

**Code Description**: 
The function _min_daily_pageviews_by_sr calculates the minimum daily pageviews for each subreddit over a specified number of days. It starts by determining the end date for the query range. If no end date is provided, it defaults to one day before the last modified date of the traffic data. The start date is then calculated by subtracting the number of days (ndays) from the end date.

The function queries the traffic data for pageviews by subreddit and path, filtering for daily intervals and specific time points within the calculated date range. It groups the results by subreddit path and calculates the minimum pageview count for each subreddit. The subreddit paths are then matched against a regular expression to extract the subreddit names, and the results are stored in a dictionary where the keys are subreddit names and the values are the corresponding minimum pageview counts.

This function is called by update_prediction_data, which uses the returned dictionary to filter and store subreddit pageview data in Cassandra. Specifically, update_prediction_data combines front page values and filters out subreddits with pageviews below a certain threshold before storing the data.

**Note**: 
- The function relies on the traffic module to fetch traffic data and the PAGEVIEWS_REGEXP to extract subreddit names from paths.
- The default value for ndays is set to NDAYS_TO_QUERY, which should be defined elsewhere in the codebase.
- The function assumes that the traffic data is available and up-to-date.

**Output Example**: 
A possible return value of the function could be:
```python
{
    'lightpainting': 16,
    'photography': 45,
    'art': 120,
    ...
}
```
This dictionary maps subreddit names to their minimum daily pageviews over the specified number of days.
## FunctionDef get_date_range(start, end)
**get_date_range**: The function of get_date_range is to generate a list of dates between a specified start date and end date, inclusive.

**parameters**: The parameters of this Function.
· start: The starting date of the range. This can be a date object or a string that can be converted to a date.
· end: The ending date of the range. This can also be a date object or a string that can be converted to a date.

**Code Description**: The get_date_range function takes two parameters, `start` and `end`, which represent the beginning and end of a date range, respectively. The function first converts these inputs into date objects using the `to_date` function, ensuring that both `start` and `end` are in a consistent format. It then calculates the number of days between the two dates and generates a list of dates by iterating from the start date to the end date, incrementing by one day at a time using `timedelta`. The resulting list includes all dates from the start date up to and including the end date.

This function is primarily used in the project to assist in date-related calculations, such as determining the range of dates for campaigns or available pageviews. For example, in `get_campaigns_by_date`, it is used to generate the set of dates for which campaigns are active, and in `get_available_pageviews`, it helps in calculating the date range for which pageview data is needed. The function ensures that date ranges are handled consistently across different parts of the project.

**Note**: Ensure that the `start` and `end` parameters are either date objects or strings that can be parsed into dates. The function assumes that `to_date` and `timedelta` are properly imported and available in the scope where this function is used.

**Output Example**: If `start` is "2023-10-01" and `end` is "2023-10-03", the function will return:
```python
[datetime.date(2023, 10, 1), datetime.date(2023, 10, 2), datetime.date(2023, 10, 3)]
```
## FunctionDef get_campaigns_by_date(srs, start, end, ignore)
**get_campaigns_by_date**: The function of get_campaigns_by_date is to retrieve and filter promotional campaigns that are active within a specified date range for a given set of subreddits.

**parameters**: The parameters of this Function.
· srs: A collection of subreddit objects or a single subreddit object. The function retrieves campaigns associated with these subreddits.
· start: The start date of the date range for which campaigns are to be retrieved. This can be a date object or a string that can be converted to a date.
· end: The end date of the date range for which campaigns are to be retrieved. This can also be a date object or a string that can be converted to a date.
· ignore: An optional parameter representing a campaign object that should be excluded from the results. If provided, the function will discard this campaign from the retrieved list.

**Code Description**: The get_campaigns_by_date function retrieves promotional campaigns that are active within a specified date range for a given set of subreddits. It first converts the input subreddits into a tuple and extracts their names. Using these names, it fetches the campaign IDs associated with the subreddits within the specified date range by calling the `PromotionWeights.get_campaign_ids` function. If an `ignore` parameter is provided, the function excludes the specified campaign from the results.

Next, the function retrieves the full campaign objects using the fetched campaign IDs by calling `PromoCampaign._byID`. It filters out any deleted campaigns that might still have associated `PromotionWeights`. The function then identifies transaction IDs associated with the campaigns, excluding those with `NO_TRANSACTION`. If valid transaction IDs are found, it queries the `Bid` table to retrieve the corresponding transactions and indexes them by transaction and campaign ID.

The function generates a set of dates within the specified range using the `get_date_range` function. It then iterates through the campaigns, filtering out those with no valid transaction or zero impressions. For each campaign, it checks if the associated transaction is either authorized or charged. If so, it adds the campaign to the result set for each date within the campaign's active date range that also falls within the specified date range.

This function is primarily used in the project to retrieve and filter campaigns for specific subreddits within a given date range. It is called by the `find_campaigns` function, which uses it to gather campaigns across multiple subreddits and their targeted subreddits. The function ensures that only valid, active campaigns are included in the results, making it a critical component for campaign management and analysis.

**Note**: Ensure that the `start` and `end` parameters are either date objects or strings that can be parsed into dates. The function assumes that the `PromotionWeights`, `PromoCampaign`, and `Bid` classes, as well as the `get_date_range` function, are properly imported and available in the scope where this function is used.

**Output Example**: If the function is called with a start date of "2023-10-01", an end date of "2023-10-03", and a set of subreddits, it might return:
```python
{
    datetime.date(2023, 10, 1): {campaign1, campaign2},
    datetime.date(2023, 10, 2): {campaign1},
    datetime.date(2023, 10, 3): {campaign2, campaign3}
}
```
Here, `campaign1`, `campaign2`, and `campaign3` are instances of the `PromoCampaign` class that are active on the respective dates.
## FunctionDef get_predicted_pageviews(srs, location)
**get_predicted_pageviews**: The function of get_predicted_pageviews is to return the predicted number of pageviews for sponsored headlines, taking into account geotargeting and default subreddit factors.

**parameters**: The parameters of this Function.
· srs: A single Subreddit object or a list of Subreddit objects for which the predicted pageviews are calculated.
· location: An optional Location object representing the geographic targeting. If not provided, the function assumes no geographic targeting.

**Code Description**: 
The function `get_predicted_pageviews` calculates the predicted number of pageviews for sponsored headlines based on the provided subreddits (`srs`) and an optional geographic location (`location`). The prediction is influenced by two main factors: the default inventory factor for subreddits and the location factor for geotargeting.

1. **Input Handling**: The function first ensures that the input `srs` is treated as a list, even if a single Subreddit object is provided. This is done using the `tup` utility, which also returns a flag (`is_single`) indicating whether the input was originally a single object.

2. **Location Factor Calculation**: If a `location` is provided, the function calculates the location factor. This factor represents the proportion of pageviews for the specified location relative to the total pageviews without any geographic targeting. The factor is computed using data from `LocationPromoMetrics` for the default subreddit (`DefaultSR`). If no location is provided, the location factor defaults to 1.0, meaning no geographic targeting is applied.

3. **Daily Inventory Retrieval**: The function retrieves the daily inventory of pageviews for the provided subreddits using `PromoMetrics.get`. This data represents the base pageviews without any adjustments.

4. **Default Subreddit Adjustment**: For each subreddit, the function checks if it is a default subreddit (using `LocalizedDefaultSubreddits.get_global_defaults`). If it is, the function applies a different inventory factor (`DEFAULT_INVENTORY_FACTOR`). Otherwise, it uses the standard `INVENTORY_FACTOR`.

5. **Final Calculation**: The predicted pageviews for each subreddit are calculated by multiplying the base pageviews by the appropriate inventory factor and the location factor. The result is then rounded to the nearest integer.

6. **Return Value**: If the input was a single Subreddit object, the function returns the predicted pageviews for that subreddit. Otherwise, it returns a dictionary mapping subreddit names to their respective predicted pageviews.

**Relationship with Callers**: 
The function `get_predicted_pageviews` is called by `get_available_pageviews` to estimate the predicted pageviews for subreddits and locations. This information is used in `get_available_pageviews` to calculate the available inventory for advertising campaigns, considering both geographic targeting and subreddit-specific factors.

**Note**: 
- The function assumes that the input subreddits are valid and that the necessary metrics (e.g., `PromoMetrics`, `LocationPromoMetrics`) are available.
- The location factor calculation relies on the availability of data for the default subreddit (`DefaultSR`). If this data is missing, the location factor defaults to 0, which may affect the accuracy of the predictions.

**Output Example**: 
If the function is called with a single subreddit and a specific location, the output might look like this:
```python
12345
```
If called with multiple subreddits and no location, the output might be:
```python
{
    'subreddit1': 10000,
    'subreddit2': 15000,
    'subreddit3': 20000
}
```
## FunctionDef make_target_name(target)
**make_target_name**: The function of make_target_name is to generate a descriptive name for a target object based on its properties.

**parameters**: The parameters of this Function.
· target: The target object for which the name is to be generated. This object must have the properties `is_collection`, `collection.name`, and `subreddit_name`.

**Code Description**: 
The `make_target_name` function constructs a name for a given target object by checking whether the target is a collection or a subreddit. If the target is a collection (i.e., `target.is_collection` is `True`), the function returns a string in the format "collection: [collection name]", where `[collection name]` is the name of the collection obtained from `target.collection.name`. If the target is not a collection, the function returns the `subreddit_name` of the target.

This function is primarily used in the `get_available_pageviews` function within the same project. In `get_available_pageviews`, `make_target_name` is called to generate a name for each target object, which is then used as a key in the returned dictionary. This ensures that the results are organized and easily identifiable by their descriptive names.

**Note**: 
- The target object passed to this function must have the properties `is_collection`, `collection.name`, and `subreddit_name`. If any of these properties are missing or incorrectly structured, the function will raise an error.
- The function assumes that the target is either a collection or a subreddit, and it does not handle cases where the target might be neither.

**Output Example**: 
If the target is a collection with the name "Tech News", the function will return:
```
"collection: Tech News"
```
If the target is a subreddit with the name "funny", the function will return:
```
"funny"
```
## FunctionDef find_campaigns(srs, start, end, ignore)
**find_campaigns**: The function of find_campaigns is to retrieve all campaigns associated with a given set of subreddits and expand the search to include campaigns targeting other subreddits linked to the initial set.

**parameters**: The parameters of this Function.
· srs: A collection of subreddit objects or a single subreddit object. The function retrieves campaigns associated with these subreddits and their targeted subreddits.
· start: The start date of the date range for which campaigns are to be retrieved. This can be a date object or a string that can be converted to a date.
· end: The end date of the date range for which campaigns are to be retrieved. This can also be a date object or a string that can be converted to a date.
· ignore: An optional parameter representing a campaign object that should be excluded from the results. If provided, the function will discard this campaign from the retrieved list.

**Code Description**: The find_campaigns function is designed to gather all promotional campaigns associated with a given set of subreddits and extend the search to include campaigns targeting other subreddits linked to the initial set. The function begins by initializing two sets: `all_sr_names` to store the names of all subreddits encountered during the search, and `all_campaigns` to store the campaigns retrieved.

The function processes the input subreddits (`srs`) in a loop. For each iteration, it updates the `all_sr_names` set with the names of the current subreddits. It then calls the `get_campaigns_by_date` function to retrieve campaigns active within the specified date range (`start` to `end`) for the current subreddits, excluding any campaigns specified in the `ignore` parameter. The retrieved campaigns are added to the `all_campaigns` set.

Next, the function identifies new subreddit names targeted by the retrieved campaigns. These new subreddit names are added to the search if they haven't been processed before. The loop continues until no new subreddits are found to expand the search. Finally, the function returns the complete set of campaigns (`all_campaigns`) associated with the initial subreddits and their targeted subreddits.

This function is primarily used in the project to gather campaigns across multiple subreddits and their targeted subreddits. It is called by the `get_available_pageviews` function, which uses it to determine the available pageviews for a given set of targets and locations. The `find_campaigns` function ensures that all relevant campaigns are included in the results, making it a critical component for campaign management and inventory analysis.

**Note**: Ensure that the `start` and `end` parameters are either date objects or strings that can be parsed into dates. The function assumes that the `Subreddit` class and the `get_campaigns_by_date` function are properly imported and available in the scope where this function is used.

**Output Example**: If the function is called with a set of subreddits, a start date of "2023-10-01", an end date of "2023-10-03", and an optional `ignore` parameter, it might return:
```python
{campaign1, campaign2, campaign3}
```
Here, `campaign1`, `campaign2`, and `campaign3` are instances of the `PromoCampaign` class that are active within the specified date range and associated with the given subreddits or their targeted subreddits.
## FunctionDef get_available_pageviews(targets, start, end, location, datestr, ignore, platform)
**get_available_pageviews**: The function of get_available_pageviews is to return the available pageviews by date for the specified targets and location, considering all equal and higher-level location targeting constraints.

**parameters**: The parameters of this Function.
· targets: A single target object or a list of target objects representing the subreddits or collections for which available pageviews are calculated.
· start: The start date of the date range for which pageviews are to be calculated. This can be a date object or a string that can be converted to a date.
· end: The end date of the date range for which pageviews are to be calculated. This can also be a date object or a string that can be converted to a date.
· location: An optional Location object representing the geographic targeting. If not provided, the function assumes no geographic targeting.
· datestr: A boolean flag indicating whether the returned dates should be formatted as strings (default is False).
· ignore: An optional parameter representing a campaign object that should be excluded from the calculations. If provided, the function will ignore this campaign.
· platform: A string specifying the platform for which pageviews are calculated. Options include 'all', 'mobile', or 'desktop' (default is 'all').

**Code Description**: 
The `get_available_pageviews` function calculates the available pageviews for a given set of targets and an optional geographic location over a specified date range. The function considers all levels of location targeting, ensuring that the available inventory is constrained by the most specific location targeting applied.

1. **Location Targeting Levels**: The function first assembles the levels of location targeting. If a location is provided, it includes the location and, if applicable, its higher-level components (e.g., metro and country). This ensures that the function accounts for all relevant targeting constraints.

2. **Campaign Retrieval**: The function retrieves all campaigns that are directly or indirectly involved with the specified targets using the `find_campaigns` function. This includes campaigns targeting the specified subreddits and any additional subreddits linked to them.

3. **Predicted Pageviews**: The function calculates the predicted pageviews for each subreddit and location combination using the `get_predicted_pageviews` function. This provides the base pageview data for further calculations.

4. **Booked Impressions**: The function determines the booked impressions for each target and location on each date within the specified range. This is done by iterating through the retrieved campaigns and summing up the daily impressions for each target and location.

5. **Available Pageviews Calculation**: The function calculates the available pageviews for each target and location by subtracting the booked impressions from the predicted pageviews. The available pageviews for a target are constrained by the most restrictive location targeting level.

6. **Platform Adjustment**: If the `platform` parameter is specified as 'mobile' or 'desktop', the function adjusts the available pageviews based on the percentage of mobile traffic (`PERCENT_MOBILE`). This ensures that the results reflect the platform-specific inventory.

7. **Return Value**: The function returns a dictionary where each target is mapped to a dictionary of dates and their corresponding available pageviews. If a single target is provided, the function returns the dictionary for that target directly.

**Relationship with Callers and Callees**:
- The function is called by `get_oversold` to determine the oversold status of a campaign by comparing the available pageviews with the daily request.
- The function relies on `get_date_range` to generate the date range for calculations, `get_predicted_pageviews` to estimate pageviews, `find_campaigns` to retrieve relevant campaigns, and `make_target_name` to generate descriptive names for targets.

**Note**: 
- Ensure that the `start` and `end` parameters are either date objects or strings that can be parsed into dates.
- The function assumes that the `Location`, `Subreddit`, and `PromoCampaign` classes, as well as the helper functions (`get_date_range`, `get_predicted_pageviews`, `find_campaigns`, and `make_target_name`), are properly imported and available in the scope where this function is used.

**Output Example**: 
If the function is called with a single target, a start date of "2023-10-01", an end date of "2023-10-03", and no location, the output might look like this:
```python
{
    "2023-10-01": 10000,
    "2023-10-02": 12000,
    "2023-10-03": 11000
}
```
If called with multiple targets and a location, the output might be:
```python
{
    "target1": {
        "2023-10-01": 8000,
        "2023-10-02": 9000,
        "2023-10-03": 8500
    },
    "target2": {
        "2023-10-01": 7000,
        "2023-10-02": 7500,
        "2023-10-03": 7200
    }
}
```
## FunctionDef get_oversold(target, start, end, daily_request, ignore, location)
**get_oversold**: The function of get_oversold is to identify dates within a specified range where the available pageviews for a target are insufficient to meet the daily request, indicating an oversold condition.

**parameters**: The parameters of this Function.
· target: A target object representing the subreddit or collection for which the oversold condition is being checked.
· start: The start date of the date range for which the oversold condition is being evaluated. This can be a date object or a string that can be converted to a date.
· end: The end date of the date range for which the oversold condition is being evaluated. This can also be a date object or a string that can be converted to a date.
· daily_request: An integer representing the number of pageviews requested per day for the target.
· ignore: An optional parameter representing a campaign object that should be excluded from the calculations. If provided, the function will ignore this campaign.
· location: An optional Location object representing the geographic targeting. If not provided, the function assumes no geographic targeting.

**Code Description**: The description of this Function.
The `get_oversold` function evaluates whether the available pageviews for a given target are sufficient to meet the daily request over a specified date range. It does this by first calling the `get_available_pageviews` function to retrieve the available pageviews for the target, start date, end date, and optional location. The function then iterates through the available pageviews by date and compares them to the daily request. If the available pageviews for a particular date are less than the daily request, that date is marked as oversold, and the available pageviews for that date are recorded in the result dictionary. The function returns a dictionary where the keys are the dates that are oversold, and the values are the corresponding available pageviews for those dates.

The function relies on `get_available_pageviews` to calculate the available pageviews for the target, considering all equal and higher-level location targeting constraints. This ensures that the oversold condition is accurately determined based on the most restrictive location targeting applied.

**Note**: 
- Ensure that the `start` and `end` parameters are either date objects or strings that can be parsed into dates.
- The function assumes that the `Location` and `Subreddit` classes, as well as the `get_available_pageviews` function, are properly imported and available in the scope where this function is used.

**Output Example**: 
If the function is called with a target, a start date of "2023-10-01", an end date of "2023-10-03", a daily request of 15000, and no location, the output might look like this:
```python
{
    "2023-10-01": 14000,
    "2023-10-03": 14500
}
```
This indicates that on October 1st and October 3rd, the available pageviews were insufficient to meet the daily request of 15000.
