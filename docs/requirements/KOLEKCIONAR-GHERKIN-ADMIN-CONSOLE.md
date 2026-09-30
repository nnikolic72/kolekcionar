# Kolekcionar.com Strategy

## High Level Plan

Initially we will do MDM module and connect it to web scraping module.

Next, we will scrape Hot Wheels Wiki at <https://hotwheels.fandom.com/wiki/Hot_Wheels>. It has comprehensive information about Hot Wheels cars and we can expand from there.

Next targets ar Pokemon TCG cards, and Magic the Gathering TCG cards.

After we create comprehensive database of collectibles, we will create Admin console that will contain the following features:

## Features

FR.1.2: Open Home Page of MDM Module

Given Admin console home page is open using /admin URL
When I click on "MDM" in Admin console menu
Then I should see a home page of MDM module
And I should see a paginated list of categories
And I should see a button "Add Category"
And I should see a button "Edit Category"
And I should see a button "Delete Category"

FR.1.1:

Feature: Add new category, manufacturer, brand, sub-brand, attributes, and attribute values

Given I clicked on "Add Category" button
When I enter "Diecast Models" in the category name field
And I click on "Add Manufacturer" button
And I enter "Mattel" in the manufacturer name field
And I click on "Add Brand" button
And I enter "Hot Wheels" in the brand name field
And I click on "Add Sub-Brand" button
And I enter "Hot Wheels Premium" in the sub-brand name field
And I click on "Add Attribute" button
And I enter "Scale" in the attribute name field
And I click on "Add Attribute Value" button
And I enter "1:64" in the attribute value name field
And I click on "Save" button
Then I should see "Diecast Models" in the list of categories
And I should see "Mattel" in the list of manufacturers
And I should see "Hot Wheels" in the list of brands
And I should see "Hot Wheels Premium" in the list of sub-brands
And I should see "Scale" in the list of attributes
And I should see "1:64" in the list of attribute values
