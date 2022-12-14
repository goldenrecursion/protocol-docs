---
description: Plugins for software and applications to disambiguate and import data to the graph.
---

# Data Plugins

We at Golden would like to encourage the community to develop and share open source plugins for commonly used software and applications that can be used to disambiguate and import data (i.e. CSVs). This enables users to utilize their application of choice to contribute to the knowledge graph. 

Plugins will be listed here after they are developed and tested. Contributors are eligible to apply for a bounty payout for their efforts. [See our incentivized testnet activities page for more information](protocol/incentivized-testnet-activities.md#developer-contributions\).&#x20;

## List of Plugins

None for now! Who will be the first to contribute?

## Plugin Requirements

Data plugins should leverage extensibility that already exist in common spreadsheet and data platforms. Users must be able to use the existing platform to upload, view, and edit a dataset. The plugin itself must expand features of the platform to:

* Map data: For example, rows to individual entities and columns to specific predicates
* Disambiguate data: Use the disambiguation service to identify existing entities, including a workflow that separates the creation of new entities and updates to existing entities
* Import data: After disambiguation is complete, users can trigger an action which creates new triples and new entities

Although non-essential, it would also be helpful to present users with core feedback in the case of failed submissions (e.g. if MDT requirements not satisfied) via an error log, alert messages, or some other form.

## Plugin Platform Suggestions

Any application that can view and edit data and supports plugins is suitable. Some recommendations are made below based on integration potential, ease of use, and popularity. 

* Google Sheets: “Add-ons” extend the functionality of Google Sheets. Add-ons are easily accessible via the [Google Workspace Marketplace](https://workspace.google.com/marketplace/search/?host=sheets) and are built via Google Apps Script. [See Google Apps Scripts’ explanation on Google Sheets Add-ons](https://developers.google.com/apps-script/add-ons/editors/sheets).
* Microsoft Excel: Either via [Microsoft 365](https://www.microsoft.com/en-us/microsoft-365/excel) or the software itself,  the functionality of spreadsheet editing can be extended through “add-ins.” [See Microsoft’s official Excel add-ins documentation](https://learn.microsoft.com/en-us/office/dev/add-ins/excel/).
* Airtable: “Custom extensions” can be created via the Blocks SDK. See Airtable’s [custom extension development page](https://support.airtable.com/docs/create-your-own-custom-extensions-with-the-blocks-sdk) as well as their [Blocks SDK site for developers](https://airtable.com/developers/extensions). 
* OpenRefine: As an already powerful open source software to view, clean, and transform data, functionality can be further extended via extensions. [See OpenRefine’s Writing Extensions page](https://openrefine.org/docs/technical-reference/writing-extensions). 
* Apache OpenOffice: 3rd party extensions can also be developed for OpenOffice software. OpenOffice is an open source project offering an alternative to paid software like Microsoft Excel. [See the Apache OpenOffice Wiki page on extensions](https://wiki.openoffice.org/wiki/Extensions).