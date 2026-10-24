# Dawaya — Egypt Medicine Guide

An English/Arabic responsive medicine directory with a blue visual theme, a blue recolor of the user-supplied pharmacy emblem framed by a red crescent, and an iPhone-friendly layout.

## Downloadable app package

For the Mac, download `Dawaya-macOS.dmg` and move Dawaya to Applications. For iPhone, download `Dawaya-iPhone-PWA.zip`, host its contents on HTTPS, then use Safari’s **Share → Add to Home Screen**.

On iPhone, the ZIP is a web app package rather than a signed native `.ipa`. Put the extracted files on an HTTPS web host, open that address in Safari, then choose **Share → Add to Home Screen**. This creates a home-screen app that launches in its own window. Apple requires signing and distribution credentials for native iPhone apps; this package does not include an `.ipa`.

## Run locally

The app fetches its public medicine data from GitHub, so it needs an internet connection. From the project folder, start a local static server:

```sh
python3 -m http.server 8000 --directory outputs
```

Open `http://localhost:8000` in a browser.

## Add to iPhone Home Screen

Host the `outputs` folder on an HTTPS site, open its URL in Safari on the iPhone, then choose **Share → Add to Home Screen**. The web manifest, Apple touch icon, safe-area layout, and service worker provide the installable app shell. The medicine catalogue still needs an internet connection; the current source data is fetched from GitHub.

## Search and medicine information

- Search by English or Arabic brand name, generic name/active ingredient, manufacturer, or listed EGP price.
- Use the language button at the bottom of the hamburger menu to switch the interface and curated medicine notes between English and Arabic. The choice is remembered on the device. The adjacent monochrome sun/moon button switches the saved light/dark appearance.
- The homepage search matches medicines and health conditions together. Its compact autocomplete dropdown groups medicine and condition matches; selecting a condition opens its detail view. The hamburger menu opens dedicated medicine/condition search and directory views, with language switching at the bottom.
- With an empty medicine search, the directory starts on the first letter and lists matching medicines; switch letters to browse the catalog. Medicine groups open a hierarchy of pharmacological subgroups; some branches, such as cephalosporins, have another level before their medicines appear. Selecting a subgroup shows a general class-level pharmacology and mechanism summary in English or Arabic, with a note that individual medicines can differ. Classification and ingredient-name matches determine group membership, so source records with unclear classifications may be placed in an "other" subgroup or may not be grouped.
- Drug records in the source dataset do not include medicine photos. Where an image URL is present, the details view shows it; otherwise the app searches Wikimedia Commons for a matching image and links to its file page. Commons results may not be an exact Egyptian product package; the original form illustration remains as a fallback.
- Medicine thumbnails load relevant image results lazily from Wikimedia Commons. Tap the image or the Show images button to open Google Images inside the app, and use the linked source caption to view the image source.
- The data comes from the [Egyptian Drug Database](https://github.com/karem505/egyptian-drug-database) JSON file and is cached for the current browser session.
- Detailed pharmacology notes are curated for a limited set of common active ingredients. The public dataset itself does not contain verified mechanisms, side effects, or administration instructions. For medicines without a curated profile, the app says those details are unavailable instead of inferring them from a broad class.
- Selecting a medicine subgroup displays an educational, class-level summary of its general mechanism and pharmacological effect in the selected language. These summaries describe broad drug classes, not the exact use, safety, or effect of an individual product; ingredient and formulation details can differ.
- The Suggestions page collects a reply email, subject, and message, then opens a pre-addressed email draft to the app owner. Because this app is statically hosted and has no mail backend, the sender must press Send in their email app; automatic delivery requires a configured server or email service.
- Medicine details also request an attributed introductory summary from the English Wikipedia API, with a source link and a note that the summary is general encyclopedia content. The app keeps its curated pharmacology notes and approved-leaflet advice separate. The medicine details include a link to search DailyMed for US product labels. Those labels may not describe the exact Egyptian product or its approved local information; the Egyptian package leaflet and local healthcare professional take precedence.
- External source links open in an in-app viewer with a **Back to Dawaya** button. Some publishers block embedded viewing; those pages can be opened in the browser from the viewer. The **Conditions** page uses the free [NLM ClinicalTables conditions API](https://clinicaltables.nlm.nih.gov/apidoc/conditions/v3/doc.html). It supports search, first-letter browsing, and body-system group browsing, and displays consumer-facing names, synonyms, ICD codes, available source links, and Commons medical image references. ClinicalTables is a terminology index, not a disease monograph or treatment guide; the cards link out to MedlinePlus, Drugs.com, and Medscape searches for further reading. Condition index names are mainly English.
- Condition details now request a linked Wikipedia article and display available lead, symptoms, diagnosis, and treatment sections in the selected interface language. Sections absent from the article are explicitly marked as unavailable; the source link remains visible. Wikipedia content can be incomplete or outdated and is educational, not medical advice. Each condition also has outbound site-specific search links for Drugs.com and Medscape. Their content is not copied into this app: Drugs.com prohibits automated scraping and dataset extraction, while Medscape limits its content license and prohibits scraping. No official public API for either site's editorial condition library was identified. Request a license/API agreement from those publishers before integrating their material.
- ClinicalTables and linked international medical sources are not Egypt-specific treatment protocols. Their content is educational; diagnosis and treatment decisions belong with a licensed clinician in Egypt.
- Route in the database describes dosage form/administration route only. The app does not provide individualized doses.
- Prices and availability can change. The directory is educational and does not diagnose or prescribe.

The source dataset is published under CC0-1.0.

## Files

- `index.html` — bilingual interface and medicine details dialog.
- `styles.css` — blue, responsive layout with iPhone safe-area support and mobile bottom-sheet details.
- `app.js` — dataset loading, autocomplete, search, Arabic/English translation, illustrations, educational profiles, combined condition search, Wikipedia summaries, and the in-app source viewer.
- `manifest.webmanifest`, `service-worker.js`, and `assets/` — installable app metadata, cached app shell, logo, and iPhone icon.

## Google Images for medicines and conditions

Medicine thumbnails are fetched from Wikimedia Commons as matching results enter view, and condition cards include a button that opens a Google Images search for the selected condition on demand. Image previews link to the publisher/source page; they are search results and may be copyrighted, so check the source terms before reuse. Search results are not diagnostic material.

The image preview lookup uses the Wikimedia Commons API without a user key. Image search buttons open Google Images in the in-app browser. The old Custom Search API key setup has been removed.

## Native download packages

- `Dawaya-macOS.dmg` contains a universal Mac app for Intel and Apple silicon (macOS 12 or later). It is ad-hoc signed but not notarized with an Apple Developer ID; on first launch, Control-click the app, choose **Open**, then confirm. It needs an internet connection for live medicine, condition, and image data. Medicine and condition suggestions open their detail views; condition details use the full MedlinePlus health-topic record returned by its Web Service, including summary text, lists, and trusted source links when available. The homepage searches both catalogs together. Wikipedia medicine summaries and an in-app external source viewer are included.
- `Dawaya-iPhone-PWA.zip` contains the iPhone web app. Host its contents on an HTTPS website, open it in Safari, then use **Share → Add to Home Screen**. A signed `.ipa` is not included: producing an installable iPhone app requires the full Xcode iOS toolchain and an Apple signing identity with a matching provisioning profile.
