# TYPO3 find configuration for Germania Sacra
This repository contains the configuration for the [TYPO3 find extension](https://github.com/subugoe/typo3-find) to display the Edfu index.


## Setup
To set up the Edfu index, you need

1. to have this repository as the `Projects/edfu` subfolder inside the find extension

2. your TYPO3 set up with:

	1. a **page** for Edfu
	2. the find extension’s **plug-in** added on that page
	3. the find extension’s **template** include added to that page
	4. the template of that page containing `<INCLUDE_TYPOSCRIPT: source="FILE:typo3conf/ext/find/Projects/edfu/edfu.ts">` to **load the TypoScript template configuration** provided in this repository

3. to have access to the Germania Sacra index (initially created from [edfu-daten](https://github.com/subugoe/edfu-daten/tree/master/#indexierung)), and the template of your page containing the **configuration to access the Solr index** created from it, e.g.

		plugin.tx_find.settings.connection {
			path = /solr/edfu
			host = my.solr.server
			port = 8080
		}

4. for **access control**:

	1. a TYPO3 frontend user group named »Edfu« set up for users who may see as-yet unpublished records, as well as the records of types »ort« and »gott«

	2. the ID of that frontend user group needs to replace the »1« in the condition `[usergroup = 1]` at the end of [edfu.ts](edfu.ts)

5. for **nice URLs**: TYPO3 should be configured to use RealURL with configuration the find extension autoconfigures. The URL for the record formular-1 should be `http://adw-goe.de/edfu/datenbank/id/formular-1`.


### TypoScript configuration
The main configuration is done in the [edfu.ts](edfu.ts) file that is included in the template.

Please refer to that file and its comments for the details.



## Templates
There are custom Index and Detail templates in `Templates/Search`. The Index Template includes the partial in Edfu/Result/ for the respective document type, the Search template includes partial in Edfu/Detail/ for the respective document type.

The data templates symlink to the default templates.


## Partials
Most subfolders with partials are symlinked to the defaults provided by the find extension. The custom partials are listed below.

### Edfu
Edfu specific partials.

#### Result
One partial for each document type. Used to create the result list items for documents of that type.

#### Detail
One partial for each document type. Used to create the detail view for documents of that type.

In addition, parts of the display logic come in their own partials:

* AnmerkungDLItem: creates a dt/dl pair with the field label and content for remarks
* AnmerkungItem: renders field content as a remark (inserting markup for transliteration font where necessary)
* BandSeiteZeile: renders the first page information and adds information about further occurrences
* Chassinat: renders the display of the Chassinat pages (page numbers, paging, hooking up the script)
* ChassinatPager: renders the pager used by the Chassinat pages display
* Photos: renders the photos for documents of type »formular«
* Stellen: renders a list of all text locations relevant for the document
* StellenDLItem: renders a single dt/dd pair of the list created by the Stellen partial
* Szenen: renders the temple map, highighling each location relevant for the current document
* Uebersetzung: renders the uebersetzung field (inserting markup for transliteration font brackets where necessary)


## Language
German and English string localisations are provided.

* locallang-edfu: Edfu specific terms of the user interface
* locallang-facets: facet names
* locallang-fields: display field labels
* locallang-form: form field labels and placeholders
* locallang: symlink to the extension’s default file


## Resources
A number of resources are included with this setup. They need to be accessed for the display to work correctly.

* edfu.css: included to style the output
	* this file is created from the edfu.scss file and should not be edited directly
* edfu.js: JavaScript handling photo display, Chassinat pager and temple plan interaction
* tempel: temple plan graphics
* Fonts: the font used to display transliterations
* fotorama: Artem Polikarpov’s [fotorama](https://github.com/artpolikarpov/fotorama/) library for the image pager
	* note that this needs to be built with grunt if checked out from github
* ddpowerzoomer.js: [Dynamic Drive’s JavaScript for zooming into the photos](http://www.dynamicdrive.com/dynamicindex4/powerzoomer.htm)
