# Google-Data-Analytics-Professional-Certificate-Summary
Putting together a summary of key information of the Google Data Analytics Professional Certificate 

## Course 1 of 8 - Foundations: Data, Data, Everywhere

## Course 2 of 8 - Ask Questions to Make Data-Driven Decisions

## Course 3 of 8 - Prepare Data for Exploration

## Course 4 of 8 - Process Data from Dirty to Clean

## Course 5 of 8 - Analyze Data to Answer Questions

### 5.1 Organizing data to begin analysis. 
  
  The four phases of analysis:
  
    1. Organize data
    2. Format and adjust data
    3. Get input from others
    4. Transform data 
    
   **Organizing Data through sorting and filtering:**
    
   *Sorting* is when you arrange data into a meaningful order to make it easier to understand, analyze, and visualize.
   *Filtering* is used when you are only interested in seeing data that meets a specific criteria, and hiding the rest. 
     
   **Filtering in SQL**
   
Filter data in SQL using the WHERE clause. For example: 
    
    SELECT
      *
    FROM
      movie_data.movies
    WHERE
      genre = 'comedy'
      
   **Sort data in a spreadsheet**
   
   When sorting data in a spreadsheet, you can choose to "Sort sheet" or "Sort range."
   
   With *Sort sheet* all of the data in a spreadsheet is sorted by the conditions of a single column, but the related information across each row stays together.
   *Sort range* doesn't keep the information across rows together. 
   
   If we want to arrange the data from the file movie_data in order of release date, then we would select the Realese Date column (in this case B) and in the menu click "DATA > Sort sheet by **column B**, A &rarr; B" keeping data together by row.   
   

 > :warning: *Heads up! The sorted results shown in the video aren't entirely accurate. Although the release dates appear to be sorted in ascending order, notice that many of the movie titles seem to be in somewhat alphabetical order, too. And that's too much of a coincidence. If you're a data analyst, you should suspect that something's not right with the data. The movie titles in Column A could have been previously sorted alphabetically and the data wasn't restored to its original unsorted format. This can happen in a real work environment.*

  If it's not important to keep the rows together and say we would like to see movie titles in afphabetical order then, we would select the Movie Title column (in this case A) then in the menu click "DATA > Sort range by **column A**, A &rarr; B". 
  
  *The SORT funtion*
  
  Lets say we have a table of party guest where there are six columns, the first two are Name and Table respectively and six rows. Say we want to sort the Names by the table number they are asigned to in ascending order (1,2, 3 etc.). We would write: 
  
    =SORT(A2:D6,2,TRUE)
  
  Now, A2:D6 is simply the range that you want to be considered. Next, write a comma to separate the range from what we're sorting by, which is column B.
Keep in mind that this part of the function **doesn't recognize column letters**. So in this case, we use the corresponding number instead, which is 2, since column B is the second column in our range. Another coma to define the order. A TRUE statement is in ascending order, and FALSE is descending. 

*A customized sort order* is when you sort data in a spreadsheet using multiple conditions. 

Like if you want to check if the guests have received an invitation or not. This can be achieved through the menu. Select the range A1:D6 then click "DATA > Sort Range". In the box select "Data has header row" which makes sure that the title of the column isn't mixed into the sorting. Then, we'll make sure it's being sorted by "Sent invitation". Here, we want the "No" responses first and the "Yes" responses second, so we'll make sure A to Z is clicked to sort the responses in that order. Because we want to add an additional sorting condition, we'll now click on "Add another sort column." The guest names should be in alphabetical order. So let's select "Guest Names" and sort from A to Z.

*Sorting and filtering in Sheets and Excel*

Excel also has the function SORTBY that by defult is in ascending order. Say we have a list of people's names (A) with corresponding age (B). If we would like to order them from youngest to oldest we would write 

    =SORTBY(A2:B9,B2:B9)
  
Notice that there is no second comma and no defining order, this is optional due to the default setting. If now we would like to add a region column (C) and also order these firstly in ascending order but the ages in descending order, we would write: 

    =SORTBY(A2:C9,C2:C9,1,B2:B9,-1)

Note that we have a 1 to indicate ascending order and a -1 for descending order.

**Sorting queries in SQL**

We can use OORDER BY to sort data in sequel. Using once again the movie data file:

    SELECT *
    FROM `movie_data.Movies`
    ORDER BY Release_Date DESC

And this will give us the list of films from newest to oldest. If we want the reverse order, just eliminate DESC as ascending order is default.

    SELECT *
    FROM `movie_data.Movies`
    ORDER BY Release_Date DESC
    
 Say now we want add a couple filters such as only being comedy movies and that made over 300M. We would write: 
 
    SELECT *
    FROM `movie_data.Movies`
    WHERE Genre = 'Comedy'
    AND Revenue > 300000000
    ORDER BY Release_Date DESC
    
  The course has a Hands-On Activity: Analyze weather data in BigQuery. Now I feel they took a bit of a leap here as there are some steps in the exercise that are not clear. The exercise says as follows
  
  *"The meteorologists who you’re working with have asked you to get the temperature, wind speed, and precipitation for stations La Guardia and JFK, for every day in 2020, in descending order by date, and ascending order by Station ID. Use the following query to request this information:*
  
  Now I wont put the whole exercise, just the first part as this is where stuff got confusing for me:
  
      SELECT
          stn,
          date,
           -- Use the IF function to replace 9999.9 values, which the dataset description explains 
           is the default value when temperature is missing, with NULLs instead.
             IF(
             temp=9999.9,         
             NULL,
             temp) AS temperature,
          -- Use the IF function to replace 999.9 values, which the dataset description explains 
          is the default value when wind speed is missing, with NULLs instead.
             IF(
             wdsp="999.9",
             NULL,
             CAST(wdsp AS Float64)) AS wind_speed,
             ...

I belive the *stn* and *date* are quite clear, but the IF seems a bit confusing. Sure the is a bit of an explanation there but its still was unclear to me. Here is what I found out.

  IF(condition, what you assined, keep the same if condition is not met), so in this case, since 9999.9 is what the data set shows when a the value is missing, then the condition is that when temp=9999.9 we assign it a value named NULL, but if it does meet this condition, then it shows the value. After all this, we then named this column AS temperature. 
  
  In the second if, notice that wdsp="999.9" and this is because this was saved in the data sheet as a string (a word, not a number). When the condition is not met, we don't want to keep using the value as a string, so when it is not NULL we CAST the value of wdsp AS Float64. What Float64 does is convert that string into a number with decimals. Say wdsp="49.3", with this CAST function it will become 49.3 in the column now assigned AS wind_speed.

### 5.2 Formatting and adjusting data

  **Convert and format data**

*From one type to another*

Sometimes, you need to convert data when you're working with spreadsheets. That might mean changing numbers into dates, strings, percentages, or even currency.

This can be done from the menu, like by selecting a column that has costs and clicking the dollar sign button to add a $ at the beginning of each number. We can also use the function CONVERT. Changing the values in column B2 from Farenheit to Celsius can be achieved by writing in a cell =CONVERT(B2,"F","C"). 

  > :bulb: Protip! Copy and paste the columns of calculated values so they no longer depend of a formula (to avoid mistakes). Simply click the column header to select all values, click copy and when pasting in another column, select "Paste Special > Paste values only"
    
*Converting data in spreadsheets*

String to date

Text to date in cell A1 in Excel: =DATEVALUE(A1)= serial number.  Serial number to date: Number Format box > date
Google Sheets: Change date format: click Format > Number > Date

String to numbers

Convert text to number in Excel: There are many ways but one first approach is to click the warning sign and select Convert to Number.
Convert text to numbers in Google Sheets: Also several ways but first approach can be by writing: =VALUE(A1)

Combining columns

Convert text from two or more cells: =A2&" "&B2 or =CONCAT(A2, " Family").
Split or combine cells in Google Sheets: To split use "Data > Split text to columns" or =SPLIT(A2," "). To combine =CONCATENATE(A2," ",B2," ",C2)

Number to percentage

Format numbers as percentages: This can be done in the menu but the cells must be expressed in decimals before applying the percentage (10 = 0.1 = 10%)
TO_PERCENT: TO_PERCENT(value) for example TO_PERCENT(1) = 100%, TO_PERCENT(0.256) = 25.6%

  *Data validation*
  
  It allows you to control what can and can't be entered in your worksheet. Uses can be: 
  1. Adding drop-down lists with predertermined options. Let's say our team has a spreadsheet that tracks everyone's progress. We can make a drop-down    menu with multiple options, like "Not Yet Started," "In Progress," and "Ready." Select the column, click "Data>Data validation." In the pop-up menuselect the "list of items" and write 'Not Yet Started, In Progress,Ready'
  2. Creating custom checkboxes. Same as before but now select "Checkbox." Write 'Approved' for "Checked" and 'Not Approved' for "Unchecked"
  3. Protecting structured data and formulas. There is option to reject invalid inputs by selecting "Reject Input" in the validation menu.

  *Conditional formating*
 
 Say we want to color code ste Status column from the previus example. Click "Format>Conditional Format", select cells to which the rule will apply. Choose "Format Cells if > Text is exactly" from the rules. For our first rule, let's write "Not Yet Started" and choose a color:red. Now all cells that have "Not Yet Started" selected from the drop-down menu will be red. Repeat for other status with other colors such as yellow and green. 
 
 *Merging and multiple sources*
 
    SELECT
     usertype,
     CONCAT(start_station_name, "to", end_station_name) AS route,
     COUNT(*) AS num_trips,
     ROUND(AVG(cast(tripduration AS int64)/60),2)
    FROM
    `bigquery-public-data.new_york.citibike_trips`
    GROUP BY
     start_station_name, end_station_name, usertype
    ORDER BY
     num_trips DESC
    LIMIT
     10
     
   Questions I have from this bit. Why if I make any changes to the GROUP BY section, it doesn't work? Can I ORDER BY a term I just defined previously in the SELECT  section? Most importantly, why is my FROM defined as `...new_york...' and not`and not `...new_york_citybike`as it appears in the bigquery-public-data table?

  *Strings in Spreadsheets*
  
 Say we have a column with the format "2020-10-31 03:03:03" and we would like to separate the date from the time. To do this, first we calculate the lengthe of the string by writing:
 
     =LEN(A2) -> 19
  
To get the position where the date begins which can be done by first finding the space (" ") between the date and the time:

     =FIND(" ", A2) -> 11
  
Therefore, the date begins at posicion 12. To obtain just the time in these cells, we can write:

    =RIGHT(A2,8)  -> 03:03:03
  
which will give us the 8 characters starting from the rights. To get the date, we can request the 11 characters starting from the left:

    =LEFT(A2, 11) -> 2020-10-31


 *Manipulate strings in SQL*
 
 CONCAT:
       
       SELECT CONCAT (‘Google’, ‘.com’);    ->    Google.com
 
 CONCATE_WS (with separator):
      
       SELECT CONCAT_WS (‘ . ’, ‘www’, ‘google’, ‘com’)   -->   www.google.com
       or
       SELECT CONCAT_WS (‘ & ’, ‘www’, ‘google’, ‘com’)   -->   www&google&com
 
 CONCAT just with '+':
    
       SELECT ‘Google’ + ‘.com’    -->   Google.com


### 5.3 Aggregating data for analysis.  
### 5.4 Performing data calculations. 


