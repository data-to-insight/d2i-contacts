---
title: D2I Contacts Processing Tool(s)
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

A browser-based process towards maintaining our members and contact records, reducing|removing our need to use Wix as core contacts management. 
Uses **stlite** to run Streamlit/pyodide(in stlite) directly in browser and embeds views via `<iframe>` containers into the html page(s).

Tool(s) available via : [Contacts Tool(s)](https://data-to-insight.github.io/d2i-contacts/)


### Problem overview

Our contacts/members have always signed up via our web platform Wix, and through the signup process gained access to both the main tools, and if relevant seperate access to such as the Early Help/Apprenticeships groups. Our outgoing contact/updates have typically gone out to our entire contacts list, or a (Wix)identifiable group within. However, as we increasingly wish to reduce both unneccessary/excess contact with some members and improve individual update quality and relevance of our, our current data(and Wix limited processing) is ineffective. 

### Problem brief

- Searches in Wix limited to full email search only
- Use of custom labelling in Wix is problematic on export/import
- Wix forces a paginated view of contacts, which as d2i base expands is increasingly slow to work with
- New/Legacy labelling within Wix means we now have unreliable single source of truth
- Filtering some specific groups e.g. Contacts-NVEST only has heavy time-cost overhead, unreliable
- Enriching contacts data is not easily possible
- Data fixes/corrects are record by record 
- Poor contacts overview enables duplicatwe records
- NEwsletter rejections can only be handled 1:1 with manual deletion in Wix 

---

## Contacts processing instructions

### Step1: Pre-processing steps

We only refresh/update the master contacts worksheet if the most recent source data is ready to bring in. Confirm/do the following before running the contacts processing tool.
  1. We need a csv copy of the master contacts list. This is because streamlit or stlite has issues with .xls file handling. So save and upload your contacts as a .csv page ready for import into the too. 

  2. Download the entire Wix contacts. The export option should be accessible via [Customers and Leads > Contacts](https://manage.wix.com/dashboard/af6cb463-8e72-4034-8f73-3641ad5abc9d/contacts?referralInfo=sidebar). Download all the contacts: 
    - `-.csv`
  2. -` 

### Step2: Tool Processing steps

Using [Contacts Processing Tool](https://data-to-insight.github.io/d2i-contacts/)

**There are x buttons/actions**
  - *Three for uploading* the needed source contacts & needed data(in this order)

    1. Single file: `\Data to Insight\Contacts - New Systems\<main contacts data as csv only>.csv`
    2. Single file: `\Data to Insight\Contacts - New Systems\<Wix latest all contacts download>.csv`   
    3. Single file: `\Data to Insight\Contacts - New Systems\zz_no_contact_remove\d2i_do_not_contact_unsubscribe.csv` 

  - *One to download* the resultant processed contacts data

    4. Data downloads as .csv file  `-////processed_contacts_refreshed.csv` 

***The processing occurs immeadiately that the 3rd upload source files are added. There is nothing to press or do in order to initiate the actual processing and the download file is available instantly after the above 3 steps are completed.***

  That's it. Whether you notice any changes or not, all the data in the contacts is now updated and current. 


---

## Important further information

### Related Tech Notes

**To do/consider list**
1. We need the logic/agreed process to handle those cases where a trust has taken over for example, or LA admin area changed in such as bradford.gov.uk vs bradfordcft.org.uk and cheshirewest.gov.uk vs cheshirewestandchester.gov.uk. 
2. Is it worth adding a 'problem identified' flag column, to make filtering possible issue reocords post-processing efficient? 
3. How to remove test records - test@d2i.org, robert.harrison@test-signup.gov.uk
4. Which flag column are we using to highlight non-LA contact? Do we need a d2i_tools column? 
5. Manual remove needed on such as socialfinnace.org.uk
6. Regarding 2. - could check domain is not-LA, but has access to EH/Tools..... 




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
- Remove unsubscribed contacts using email match
- Standardise casing of fields like `first_name`, `local_authority`, `role`
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
- [stlite](https://github.com/whitphx/stlite) to run Streamlit app directly in browser (WebAssembly + Pyodide)  
- [Bootstrap 5](https://getbootstrap.com) for layout  


## Dev notes & testing

- At the moment py code is embedded into index/upload_tool page, so can't be tested via usual streamlit local running. Instead run:
python -m http.server 8000, or select a different port/py: python3 -m http.server 8501

---

## Deployment/Development notes

Built to run in **GitHub Pages**. 
Note: avoid direct browser refresh on subpages (e.g., `/upload_tool.html`) unless served (e.g., via iframe or root-relative path) - as i've not added # or 404 handling

---
