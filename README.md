# Biocard Frontend Code Challenge

## Setup
### Requirements
This project was developed on Windows with Node `12.9.0`. Everything *should* be environment-agnostic, though.

### Getting Started
To install all required packages and start serving the application, run the following commands:
```
npm install
npm run serve
```

## Summary
This was my first time using Vue, but I already had a bit of react/typescript experience, mostly from creating WordPress Gutenberg/BB blocks.

## Components
### JSON
All content on the page is generated from the 2 json files in `/src/assets`:
#### `categories.json`
The exact same json from the test PDF.

#### `biometrics.json`
An expanded version of the json from the test PDF. I added more entries to recreate everything shown in the PDF's screenshot. I also added one new field: `intensity`.

### HomeView
Not a component, but worth mentioning for completeness! Initializes one `CategoryList` component, then one `CardCategory` for each entry in `categories.json`.

### CategoryList
Displays a styled, unordered list of categories based on the json that it's passed. Also sets up a click handler for each category in the list.

### CardCategory
Creates child `BioMetricCard` components based on the contents of `biometrics.json`.  Counts the `intensity` fields of each biometric that belongs to the category so that we can display the "stoplight" in each category header. If there are no biometrics with a `categoryId` that matches the category's ID, nothing is rendered.

### BiometricCard
Mostly renders whatever entry from `biometrics.json` it's given. It also does some processing of the `description` field to see if there is a number in it. If so, the number is colored based on the intensity of the biometric.

Initially, I tried to derive the `intensity` field entirely from the json fields in the PDF. I planned on extracting the number from each description and setting arbitrary breakpoints for how far off the number was from the recommended level (20+ above the rec. level: *Critical*, 5+ above the rec. level, *Concern*, etc.).
In the end, there were too many unknowns:
* Waist Circumference and Existing Heart Disease from the example both don't have a number in their descriptions, but they have different Intensities
* Card values can have different units. A difference of 20 can be a huge deal or a minor issue depending on the units used!

With that in mind, adding a new field made the most sense.


### Potential Improvements
* Category List
    * Clicking a category at the top could scroll you to the Category on the page.
    * Alternatively, you could only show one category at a time and clicking on a category in the list would show that category.
* Load More button
    * Could cap the number of displayed cards/category at 2 or 4, then clicking would show more/all of them.
        * You'd also want to sort the cards by severity so that the important stuff always comes first.
* Take Action button
    * Obviously this should do something, too. This one is the most vague because it's completely dependent on business logic.
    * Could have an `actionUrl` json field that takes you somewhere else when you click the button.

## Screenshots
### Desktop
![Desktop Screenshot](/public/ss-desktop.png)


### Mobile
![Mobile Screenshot](/public/ss-mobile.png)