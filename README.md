# df-mod6-mobile

Mobile device forensics using SQLite

- [GitHub](https://github.com/denisecase/df-mod6-mobile)

This respository includes files used in the Digital Forensics Challenge 
described at [TSPD.md](TSPD.md).

## Tools

- VS Code
- VS Code Extension: SQLite Viewer by Florian Klampfer
- [DB Browser for SQLite](https://sqlitebrowser.org/)

To install an extension in VS Code, 
go to the Extensions tab and search for the name and author. 

Viewing tables in VS Code (or DB Browser) makes it easier to search and explore the organization.

## Open Example Mozilla Broswer Evidence Folder

We have an example of Mozilla Firefox Browser Evidence Folder in the TSPD folder.

Clone, or fork and clone this repo down to your machine. 
Look for the [TSPD/j3uv3vkf.default/](TSPD/j3uv3vkf.default/) folder. 


This database includes 13 tables. 

For example, there is a  `moz_places` table in the places.sqlite database. 
This table contains information about the user's browsing history, 
including the URLs visited and the date and time of the visit.

## Explore Databases in VS Code

You can browse and search the database files in VS Code by clicking on them with the recommended extension installed.

Click on a .sqlite file (pink icon) to open the table and search the data.

## Explore Databases in DB Browser for SQLite

## My Investigation Findings

After reviewing the 'places.sqlite' file using DB browser using SQLite, I discovered the user had a interest in stuffed animals and also Disney products.

### Evidence


## When I went to the EXECUTE SQL filter and filtered for '%amazon%' I was able to make the connection that the user used firefox to access amazon.

## addons.json

The addons.json helped to find valuable information about this person including the following: Extension name and ID, Developer information, Description and permissions, and installation and update URLs. This insight added some context into the data found in my other sqlite tables.

During my investigation I went to healthreport.sqlite file which had data from the browser. Goal being to identify user interaction within the amazon related content. I then went under browse data to see available tables and typed in the final command of '%amazon%'

These results showed me this person used firefox to access amazon.

## cleanup.sqlite

I was able to use the information to make inferences given the information cleared or retained by this individual.

## Databases (.sqlite) and Tables

Each .sqlite file is a database. A database can have many tables. 
For example, there is a database file named 
`places.sqlite`. 

## Form History

To uncover personally identifiable information (PII) relevant to the TSPD case, I analyzed the formhistory.sqlite database — specifically the moz_formhistory table. This database contains browser form autofill entries, making it a rich source for data entered into fields like names, addresses, emails, and payment info.

Using DB Browser for SQLite, I queried and examined the contents of this table and found 11 key records that matched the profile of the suspected buyer, Keyser Soze. The fieldname and value columns revealed the subject's:

Full Name

Cell Phone Number

Email Address

Company

Mailing Address

Job Title

Credit Card Info

I confirmed the legitimacy of this data by cross-referencing timestamps (firstUsed, lastUsed) and verifying it was consistent across multiple browser sessions.

Supporting Evidence from Other Artifacts:
addons.json: This file indicated the presence of the Form History Control extension, which preserves form data even after routine browser cleanups. This suggested that form history retention may have been deliberate or configured not to expire.

cleanup.sqlite: While reviewing this database, I found schema fields that track deletion events and cleanup schedules. There was no matching entry suggesting that this form data had been purged, further reinforcing its authenticity.

healthreport.sqlite: Provided insight into browser usage patterns and plugin behavior, confirming the likelihood of the form fields being used frequently on this device.

Finally, I exported the moz_formhistory table to a CSV file (moz_formhistory.csv) for submission, preserving all relevant entries and their associated metadata (timestamps, GUIDs, etc.) for transparency and reproducibility.

**Disneystore.com**

Regular visits to **Disney.com** which includes searches and checkouts

Searches via google of: " stuffed animal toys, Disney plush, global stuffed animals"

URLs directed towards:

**ebay.com** stuffed animal listings
**globalstuffedweebly.com**
Cleveland Metroparks Zoo gift shop

This evidence above suggests the user of this laptop was involved in the trafficking of stuffed animals that was referenced in this conversation.

In DB Browser, use  File / Open Database to open a database file. 

Use the drop-down list to select a table in the database. 

Use the "Execute SQL" tab to run queries. 

For example, you can run a SELECT statement to retrieve data from a table:

```SQL
SELECT * FROM moz_places;
```

We can select just some of the columns - or limit our search to urls that contain http or https.

```SQL
SELECT url, title FROM moz_places;
SELECT url, title FROM moz_places WHERE url LIKE '%http%';
```

Investigators can use SQL queries to search for other types of information, 
e.g., credit card numbers and email addresses. 

We can join tables together by matching fields and using the keyword `JOIN`.

For example, you can use the following query to search for email addresses:

```SQL
SELECT url, title FROM moz_places JOIN moz_bookmarks ON moz_places.id = moz_bookmarks.fk WHERE content LIKE '%@%' AND url LIKE 'http%';
```

This query will search for email addresses in the URL or page 
content of websites that start with "http". 

After running a query, use the "Save the results view" icon (to the right of the red circle x ) to save as a CSV file.

Example: out.csv

## Optional SQLite CLI and PowerShell

To install SQLite CLI, see: 
https://www.sqlitetutorial.net/download-install-sqlite/
Download a file similar to: 
sqlite-tools-win32-x86-3410200.zip
(1.91 MiB).
Extract the files and move them as needed, so you have 3 .exe files in C:\sqlite.

Once the CLI is installed, you can use PowerShell and PowerShell scripts to execute SQL. 
See get_places1.ps1 for an example. To try it, open a PowerShell terminal and run:

```PowerShell
.\get_places1.ps1
```

Verify that out1.csv is created. 

## Resources

- [SANS DFIR Advanced Smartphone Forensics](https://www.sans.org/posters/dfir-advanced-smartphone-forensics/)
- [Dataset construction challenges for digital forensics, 2021](https://tsapps.nist.gov/publication/get_pdf.cfm?pub_id=931168)
- [GitHub: Android Forensics Resources](https://github.com/RealityNet/Android-Forensics-References)
- [University of New Haven DATASETS FOR CYBER FORENSICS](https://datasets.fbreitinger.de/datasets/)
