# Splunk

## Using Splunk Web
Administrator Role: install ap ingest data create knowledge object
Power Role: create and share knowledge oject of an app
User role: can see only created their knowledge object and the one shared with them

Search Bar: it is able to show the data in a certain interval of time.
-   Data Summary:
    - Source Type: classification of data
    - Sources: directory port or networkpath
    - Host: domain name or machine where the data are created

## Using Splunk Search
Limit a search by time is **key** a search page open, use a search will display event.
-   Event list and the fields extracted in the events are dispalyed in each field.
- Job is a specific search done that can be eliminated or inspected and shared. A search Job is for defalut saved for **10 min** instead a shared search job last for **7 days**.
- From a job we can choose 3 different mode:
    - Fast Mode: Field discovery is disable 
    - Smart Mode: will toggle the event based on the search
    - Verbose Mode: All the field are displayed

## Exploring Event
- Reverse chronological order are given for the events
- And the time is given, by the local machine time
- failed password shows (failed password)

## Using Search Term
- fail* (fail+any) 
- booleans (fail NOT password) OR AND (AND is space fail password) can be used
- exact phrase can be use quotes "failed password"
- looking for results that contain quotes use the \" ..."\

## What are the commands?
Splunk is build on 5 components:
- Search Terms
- Commandas what we want to do with the search
- Function how do we want to create the graph 
- Arguments name appyed to the graph 
- Claused how we want them group and defined

Eg. 
index=network sourcetype=cisco usage=Violation         #Search Terms
| status count(usage) as Visits #Next Quiry (|) Command(status) Function(count) Argument(Argument) Clause(as)
| search Visists > 1

## What are knowledge objects?
Knoledge objects are grouoed in 5 categories:
-   Data interpretation (fields in the event)
-   Data Classification (event types)
-   Data Enrichment (lookups)
-   Data Normalization (tags to more data)
-   Data Model (hyrierciacal )

knowledge object are important because they can be shared between user and used again by multiple people and apps.

# Using Field
Most important are **Selected Fields** that are the bottom of each event (host, source, source type).
**Interesting Fields** are field that are shown at leat in the 20% of the events. an **a** denote a string value instad a **#** denotes a numeral. 
Click on the field will show the **Values** for the filed and the count of each value for the filed

## Fields in Search Quaries
I can write this in the source bar:
- sourcetype=linux_source (fileds on the left are case sensitive instead the right side it is not).
- host!="mail*"
- I can use also the boolean NOT OR AND
- Important != is not the same as NOT operator
- als IN can be use to include different or one string in the search bar
- |fields status is anothe way to filer by status and **rememebr filter before do operation is importat**
- |fields -status included +status exclueded
- |rename status as "HTTP Status" 

## Fields in Search Results
- Temporany Filds using | eval so we are looking for field but we don't like how they are shoed example byte is not really meanigfull we prefere to show it in MB. So we use
|eval bandwidth = Bytes\1024\1024
so bandwith is created 
- Filed extraction:
    -   |rex regular expration |rex filed=_raw "insert regular expression"
    -   |erex Character fromfield=_raw examples="pixie, Kooby" # in example we shows some results that it should find

## Enriching Data with Knowledge Objects
-   | eval bandwidth = Bytes/1024/1024
this make our results much better to read.
But creating a calculating filed will do it automatically each time we use the search bar
-   Fileds alias create fields of similar fileds with different name. so we can refere to the new fileds with the alias or the old names of the fields
-   Important for field: Filed extraction --> Field Aliases --> Calculated Field --> lookups --> Event Types --> Tags

## Links
http://docs.splunk.com/Documentation/Splunk/latest/Knowledge/Aboutsplunkregularexpressions.
https://www.youtube.com/channel/UCjwOFZzLPnji1EstaVyyvAw
https://docs.splunk.com/
https://youtu.be/QvxLJ9f6AK0

# Splunk Using Fileds Slides
## What are Fields?
- Fields are knowledge objects that represent searchable **key/value** pairs in your event data, e.g. *host=www1* and *status=503*
- Fields can be assigned to or expected from events at various times in the data pipeline and search process
## Field Auto-Extraction: Before Search 
- Prior to search time, some fields are already store in te event index, suach as:
        -   IMPORTANT: host, source, sourcetype
        -   Other: index, linecount, putc ecc ecc
        -   Custom index fields
        -   Internal Fields
## Field Auto-Extraction: At Search 
-   At search time, Splunk automatically looks for **key=value** patterns and extracts them into field-value pairs for events associated wirh specific host source and sourcetype
## Filed Auto-Extraction
We have 2 object, the indexer, events have **default fields** and **coustum indexed fields** prior to search time, the second object is the **Search Head** additional **search fields** and custom search fields extraction are added to events aat search time, this to objects comunicate between each other to return the **Events**.

## Extracted Fields are Knowledge Objects
-   Knowledge objects provide specific information about your data
-   Extracted fields are executed first in search time operations
-   Field aliases and calculated fields are custom fields that can add additional context to and build on extracted fields

## Look to Fields Side Bar (left side)
-    **Selected Fields**: a set of fields displayed for each event
-    **Interesting Fields**: occur in at least 20% of resulting events
-    **All Fields**: link to view all fields (including non-interesting fields)
N.B. **#** rapresent numeric field instead **a** rapresent character fileds

### Selected Fields 
Selected fields are listed under every event that includes those fields, defaults selected fields: **host**, **source**, **sourcetype**.
Add a fields to selected Field List just click on the fields on the event a pop-up will show and we can just press yes/no to display in selectd those fields. From **All Fields**, you can indentify other fields as selected fields or change the event covrafe amount. This butto in present in the upper left sidebar. Furthrore in the Windows filelds we cab see some simple statistic that we can easily display.

### Index in (Selected Field)
Index always display in selected fields if no index is indicated in the search, data is returned from all default idexesthe role of the user executing the search. (best practice always sepcify the index).

## What is field discovery?
At search time, Field dicovery extract fields from eaw event data, including those directly related to the search's results.
-   Identifies and extracts the first 100 obvious key=values pairs in the raw event data
-   Extracts any field explicitly mentioned in the search
-   Performs Custom field extractions defined by the user
-   Field Discovery is based on the searched sourcetype, as well as **key=value** pairs found in the data

## Search Modes and Field Discovery
The Splunk user interface provides 3 search modes. Search modes determine how much field data is returned as search results, and affects how fast the search completes:
-   **Fast Mode**: Prioritizes speed over completeness, disables Field Discovery and returns default fields and indexed field extractions. It extracts and returns specific fields if hose fields are specified in the basic search, if a transforming search is run, the
results display on the Statistics or Visualizations tabs only.
-   **Verbose Mode**: Prioritizes completeness over speed returns all extracted fields . returns full event list and event timeline for every search. slowest search mode due to
increased size of search payload.
-   **Smart mode**: Default search mode, balances speed and completeness, if a transforming search is run, user
is taken straight to report result table or visualization:
No event list or timeline is generated. Behaves like Fast Mode
If a non-transforming search is run: Event list and timeline is generated. Behaves like Verbose Mode.

## Use Fields in Search:
Fields provide an efficient way to filter events and refine search results.
-   basic search:
    -  clintip=141.364.3.65 
    -  area_code=404
    -  VendorCountr="United States" (in quotes look for exact matching string in the quotes).
-   Case Senstive **(fields name are case sensitive, fields value are not)**:
    -  host=ww3 == host=WW3 != HOST=ww3
-   Use Fields in Searchs WildCards
    -   user=* sourcetype=access* (referer_domain=\*.cn OR refe=\*.hw)
-   Field Expression:
    - =, !=, <, >, <=, >=
-   Operators:
    -  OR 
    -  IN
    -   AND (and is seen as sapace) host=1 status=404 == host=1 AND status=404
    - != is not the same of NOT status!=200 return elements where status field exists and value in field doesn't equal 200 instead NOT stats=200 returns events where the status field exists and value in field doesn't equal 200 and all events where status field does not exist.
- Commands:
    - **rename**: |rename \<field\> AS \<new-field\> (rename the dield)
    - **table**: |table \<field\> \<field\> ... (show the table based only on the selected fields)
    - **field**: |fields [+|-] [\<field-list\>] (add or remove from interesting fields the fields listed)
    - **count**: |count \<field\> 
    - **sort**: |sort \<field\>

## Temporary Fields versus Persistent Fields
-   Temporary fields:
    - Exist for the duration of the search
    -   Can be created from search results with the eval command
    -   Can be extracted from search results with the erex or rex command
-   Persistent fields:
    - Are knowledge objects that can be shared
    - Are created by users, knowledge managers, and admins
- Command (Persistent):
    - |eval \<field1\> = \<expression\> (eval bandWidth = bytes/1024)
- Command (Temporary):
    - |erex \<field\> examples="\<example1\>, \<example2\>, \<example3\>, ..."
    - |rex field=\<field\ "\<regular-expression\>"
    - |rex field=\<field\ mode=sed "\<regular-expression\>" (need to modify also the field with the regex) examle: rex field=account mode=sed "s/(\w{4}-)\S+/|1xxxx/g" / are the separetor of the regex, the first field is s and stends for replace y for substitute, the second field is the regex, the third is the replacement field, the fourth is the flag g stends for gloabl

## Enrich Data
Sometimes statistics data are required  but isn't present in the row event data, the **Lookups** pull such data from standalone files at search time and add it to search results as fields.
A way to associate as additional new name with an existing field is by using the **field aliases** can be used to normalize field names, they can be referenced by knowledge objects tat suceed aliases in the search process pipeline.
**SEARCH PROCESS PIPELINE**:
- Extractions
-   Field aliases
-   Calculated Fields 
-   Lookups
-   Event Type
-   Tags

# Visualization
-   searching:
index=web sourcetype=acces_combined product_name=*
|fields - product_name price (exlude them form the field name) 
|fields + product_name price (include them in the interesting field)
|table JSESSIONID product_name price (show this in table)
using them in order to imporve the fast.
-   dedup:
|fields + product_name price (include them in the interesting field)
|table JSESSIONID product_name price
|dedup JSESSIONID (eliminate duplicate)
-   addtotals:
|chart sum(price) over product_name by VendorCountry
|addtotals col=true label="Total Sales" labelfield="product_name" fieldname="Total by product" row= False











