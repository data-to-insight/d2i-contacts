---
title: D2I Contacts Tool
authors:
- Rob Harrison
status: draft
last_updated: '2025-06-10T08:34:43Z'
tags:
- d2i
- loader
- validation
tech_stack:
- python
- stlite
- excel
- csv
---


# Contacts processing to maintain d2i contacts

A browser-based process towards enabling and maintaining a clean(er) and reliable members and contact record; reducing|removing our need to use Wix as core contacts management. Uses **stlite** to run Streamlit/pyodide(in stlite) directly in browser and embeds views via `<iframe>` containers into the html page(s). Currently only one tool is defined/requested but this might be extended to include others as needed. 

Tool(s) available via : [Contacts Tool(s)](https://data-to-insight.github.io/d2i-contacts/)


## Problem

Our contacts/members continue to sign up via our web platform Wix, and through the signup process gain access to both the main tools, and if relevant seperate access to such as the Early Help/Apprenticeships groups. Our outgoing contact/updates/newsletters have historically/typically gone out to our entire contacts list, or a (Wix)identifiable group within. However, we increasingly want to reduce both unneccessary/excess/invalid contact with some members and improve individual update quality and relevance of it. Our current data(and Wix limited processing) is proving to be problematic, uncertain and has a bloated time-cost overhead for simple tasks(incl. filtering). 

One solution is to clean the existing data and address the shortfalls within Wix itself. However within this approach is the potential for introducing new problems specifically around tool members access tied to Wix internals incl. contact labelling, and the poor time-efficiency when handling contacts via the Wix interface(e.g. paginated views only) as well as the difficulty around reliably grouping contacts - and access to user defined groupings in exports. 

We have made a long-term plan to move away from Wix site to a clean d2i site build(most likely using Django), and as such **this tool supports any interim move to manage contacts outside of Wix, but still maintain/refresh members details from Wix** (as this remains our members' single sign-up entry point).

### issues summarised

- Searches in Wix limited to complete-email|name only
- Use of custom labelling in Wix is problematic on export/import
- Wix forces a paginated view of contacts, which as d2i base expands is increasingly slow to work with 
- New/Legacy labelling within Wix means we now have unreliable single source of truth
- Filtering some specific groups e.g. Contacts-NVEST only has heavy time-cost overhead, unreliable
- Enriching contacts data is not easily possible
- Data fixes/corrects are record by record 
- Poor contacts overview enables duplicate records
- NEwsletter rejections can only be handled 1:1 with manual deletion in Wix
- Re-imports back into Wix problematic, custom field loss

---

## Contacts processing tool instructions

### Step1: Pre-processing steps

Confirm/do the following before running the contacts processing tool:

  1. Download a fresh copy of the entire Wix contacts. The export option is accessible via [Customers and Leads > Contacts](https://manage.wix.com/dashboard/af6cb463-8e72-4034-8f73-3641ad5abc9d/contacts?referralInfo=sidebar). The file can be named anything suitable, but recommend a date for ease of reference `280625.csv`

  2. This should be available by default, but just for clarity we'll be using, and refreshing the **csv** copy of the current 'master contacts list'(==D2I offline main contacts sheet). (Note...has to be CSV because streamlit or stlite appears to have issues with .xls file handling, so everything going into the into the tool needs to be .csv format.)


### Step2: Tool Processing steps

Using [Contacts Processing Tool](https://data-to-insight.github.io/d2i-contacts/)

**There are 3 buttons/actions**
  - *Three for uploading* the needed source contacts & needed additionl data(in this order)

    1. Single file: `\Data to Insight\Contacts - New Systems\<main contacts data as csv only>.csv`
    2. Single file: `\Data to Insight\Contacts - New Systems\<Wix latest all contacts download>.csv`   
    3. Single file: `\Data to Insight\Contacts - New Systems\zz_no_contact_remove\d2i_do_not_contact_unsubscribe.csv` 

  - Then *One to download* the resultant processed contacts data

    4. Processed contacts data downloads as .csv file, e.g.  `/processed_contacts_refreshed.csv` 

***The processing occurs immeadiately that the 3rd upload source files are added. The user does not need to press or do anything in order to initiate the actual processing and the download file is available instantly after the above steps 1-3 are completed.***

  That's it. Whether you notice any changes or not, all the data in the contacts is now updated and current. 


---

### Filter contacts list for newsletters

| Column           | Filter Description                                                                 |
|------------------|-------------------------------------------------------------------------------------|
| duplicate        | Contacts flagged as possible duplicates under separate email addresses             |
| d2i              | `Y` = Receives D2I newsletter                                                       |
| d2i_tool_access  | `Y` = Has .gov.uk LA email or agreed tool access                                   |
| nvest            | `Y` and `d2i = N` = Receives only NVEST newsletter                                 |
| send             | `Y` = Identified as SEND contact (if populated)                                    |
| apprenticeships  | `Y` = Historic or current apprentice                                               |
| early_help       | `Y` = Has access to Early Help                                                     |
| cms              | CMS type for ref                                                                   |
| new_contact      | `Y` = Identified as new contact in current or last import process (e.g. from Wix)  |


--- 

## Important further information

### Related Tech Notes

### Columns in contacts file

*Short-term*, we 're retaining all the Wix default export columns.... these are not expected to form part of the onward/finished contacts output or processes. 

**all current columns**
first_name,last_name,domain,email,preferred_title,job_title,dcs_or_senior_role,duplicate,d2i,d2i_tool_access,nvest,send,apprenticeships,early_help,cms,label,email subscriber status,last_activity,last_activity_date,source,importlabels_d2i,new_contact,email_2,phone,city,country,importlabel_cms,urn,la_code,la_code_new,local_authority,region_code,region_name

**longer-term expected end columns(order tbc)**
| Column             | Description / Use                                                                  |
|--------------------|-------------------------------------------------------------------------------------|
| first_name         | single or multiple forenames                                                       |
| last_name          | single or multiple sur/last names                                                  |
| domain             | Email domain, used for matching to LA                                              |
| email              | Primary email address (should be unique within this list)                          |
| preferred_title    | Title preference, incl. Mr, Ms, Dr...                                              |
| job_title          | Job title or role                                                                  |
| dcs_or_senior_role | Flag if Dir of Children’s Services or other senior leadership role                 |
| duplicate          | Internal flag for possible duplicate entries with different emails                 |
| d2i                | D2I contact (no assumption of tool access privileges)                              |
| d2i_tool_access    | Indicates access rights to D2I tools (has LA address or agreed access)             |
| nvest              | Flag as NVEST contact                                                              |
| send               | Flag as SEND contact                                                               |
| apprenticeships    | Flag as registered current or historic apprenticeship contact                      |
| early_help         | Flag as Early Help contact                                                         |
| urn                | Unique Reference Number (if applicable)                                            |
| la_code            | LA code                                                                            |
| la_code_new        | New standardised LA code (E000000 format)                                          |
| local_authority    | Local authority name (as per Ofsted naming)                                        |
| region_code        | Region code (e.g. E12000007)                                                       |
| region_name        | Full name of region (e.g. London, North West)                                      |
| cms                | Case Management System (CMS)                                                       |
| new_contact        | Flag, default Y for a new WIX signup within this process run, enables manual chks  |


**To do/future list**
1. We need the logic/agreed process to handle those cases where a trust has taken over for example, or LA admin area changed in such as bradford.gov.uk vs bradfordcft.org.uk and cheshirewest.gov.uk vs cheshirewestandchester.gov.uk and add them to the la_reference_data file + code. 
2. Is it worth adding a 'problem identified' flag column on the output file, to make filtering possible issue reocords post-processing efficient? 
3. How to remove test records - test@d2i.org, robert.harrison@test-signup.gov.uk - we cannot just search for 'test' as might give false positives.
4. Which flag column are we using to highlight non-LA contact? Do we need a 'd2i_tools' column or use existing 'd2i'? 
5. Manual remove needed on such as socialfinnace.org.uk
6. Regarding 2. - could have a check for when domain is not-LA, but has access to EH/Tools, have some similar name chaecks with different la address(people move la and dont update)



### Process Logic

- Upload **main D2I contacts**, optional **Wix contacts**, and **unsubscribe list**
- Clean and rename columns to a standard format (e.g. `first_name`, `email`)
- Drop unused or duplicate columns (e.g. `email 3`, `labels`)
- Normalise email addresses (lowercase, strip spaces) and extract domain
- Deduplicate both `df_main` and `df_wix` by keeping:
  - The most **recently active** Wix contact (`last_activity_date`)
  - Or the most **complete record** (highest number of filled fields)
- Merge new Wix contacts into main, tagging with `new_contact = 'Y'`
- Apply LA reference data (`URN`, `LA codes`, `region info`) using domain as key
  - LA reference data is hard-coded containing : `domain`,`urn`,`la_code`,`la_code_new`,`local_authority`,`region_code`,`region_name`
  - LA reference data original file is located `\Contacts - New Systems\Admin-Other\` for off-line reference
- Remove unsubscribed|no-contact contacts using email match from uploaded **unsubscribe list**
  - The master no-contact list is stored locally at: `\Contacts - New Systems\Admin-Unsubscribe-NoContact\*.csv`
- Standardise casing of fields `first_name`, `local_authority`, `role`
- Reorder columns for export, placing core fields at the front
- Fill all missing/null/blank values for Excel compatibility
- Export a clean downloadable CSV (`Download Merged Contacts`)
- Show a **green-highlighted preview** of newly added Wix records
- Display domain-level contact counts for analysis


### Still to do

- Can we improve f/s name and other field normalisation? (basic text quality|fixes to apply to all)
- Title case on all values or some? 
- Add optional upload for NVEST registered so we can auto flag in main contacts direct from newest data?
- Clarify the EH etc labelling, is it accurate and can this also be used for auto-labelling (same for SEND?)
- Do we need seperate/other related tools here, consider if refactor towards that a good idea. 

---


## Repo structure

- `index.html`:  
  Landing page, currently with: 
  - Left: `upload_tool.html` (iframe)  

- `upload_tool.html`:  
  HTML file embedding a stlite-powered Streamlit app

- `assets/`:  
  Img assets (e.g., `images/d2i_logo.png`)
  Styling assets (e.g., `css/styles.css`)

---

## Tech stack

- [Streamlit](https://streamlit.io) for data interface  
- [stlite](https://github.com/whitphx/stlite) to run Streamlit app directly in browser (Pyodide)  
- [Bootstrap 5](https://getbootstrap.com) for layout  


## Dev notes & testing

- At the moment py code is embedded into index/upload_tool page, so can't be tested via usual streamlit local running. Instead run:
python -m http.server 8000, or select a different port/py: python3 -m http.server 8501

---

## Deployment/Development notes

Built to run in **GitHub Pages**. 
Note: avoid direct browser refresh on subpages (e.g., `/upload_tool.html`) unless served (e.g., via iframe or root-relative path) - as i've not added # or 404 handling

---
