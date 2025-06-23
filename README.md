---
title: README
authors:
- Rob Harrison
status: draft
last_updated: '2025-06-10T08:34:43Z'
tags:
- loader
- validation
tech_stack:
- python
- stlite
- excel
- csv
---


# Contacts tool/simple interim processing to maintain d2i contacts

In-progress collection of tools towards contacts related processing.

Tool(s) available via : [NVEST Tools](https://data-to-insight.github.io/d2i-contacts/)


## Contacts processing tool

A browser-based process for maintaining contacts.
Uses **stlite** to run Streamlit/pyodide(in stlite) directly in browser and embeds views via `<iframe>` containers into the html page(s). 
---

## Instructions

### Step1: Pre-processing steps

The master contacts worksheet. will only be up to date if the source data is. Confirm/do the following before running the contacts processing tool.

  1. Download the entire Wix contacts form submissions. The NVEST contacts, download should be accessed only via [Forms and Submissions](https://manage.wix.com/dashboard/af6cb463-8e72-4034-8f73-3641ad5abc9d/wix-forms-and-payments). Download this data into : 
    - `-.csv`
  2. -` 

### Step2: Tool Processing steps

Using [Contacts Processing Tool](https://data-to-insight.github.io/d2i-contacts/)

**There are x buttons/actions**
  - Three for uploading the needed source contacts & needed data(in this order)

    1. Single file: `-.csv`

    2. Single file: `-.csv`   

  - One to download the resultant processed contacts data

    - Data downloads as .csv file but MUST be saved/**overwrite** `-////processed_contacts_refreshed.csv` 

***The processing occurs immeadiately that the 3rd upload source files are added. There is nothing to press or do in order to initiate the actual processing and the download file is available instantly after the above 3 steps are completed.***


  That's it. Whether you notice any changes or not, all the data in the contacts is now updated and current. 



---

## Important further information



## Related Tech Notes

### Process Logic



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

---

## Deployment/Development notes

Built to run in **GitHub Pages** and other static hosting providers. Ensure:

- avoid direct browser refresh on subpages (e.g., `/upload_tool.html`) unless served (e.g., via iframe or root-relative path) - i've not added # or 404 handling
- Test : python3 -m http.server 8501

---
