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






