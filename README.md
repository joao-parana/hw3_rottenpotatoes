Iniciando com o exercicio
=========================


imperative_bad_scenario.feature
===============================

    Feature: movies should appear in alphabetical order, not added order
    
    Scenario: view movie list after adding 2 movies (imperative and non-DRY)
    
      Given I am on the RottenPotatoes home page
      When I follow "Add new movie"
      Then I should be on the Create New Movie page
      When I fill in "Title" with "Zorro"
      And I select "PG" from "Rating"
      And I press "Save Changes"
      Then I should be on the RottenPotatoes home page
      When I follow "Add new movie"
      Then I should be on the Create New Movie page
      When I fill in "Title" with "Apocalypse Now"
      And I select "R" from "Rating"
      And I press "Save Changes"
      Then I should be on the RottenPotatoes home page
      And I should see "Apocalypse Now" before "Zorro"
    


declarative_good_scenario.feature
=================================

Feature: movies when added should appear in movie list

Scenario: view movie list after adding movie (imperative and non-DRY)

  Given I have added "Zorro" with rating "PG-13"
  And   I have added "Apocalypse Now" with rating "R"
  Then  I should see "Apocalypse Now" before "Zorro" on the Rotten Potatoes home page




declarative_steps.rb
====================

Given /I have added "(.*)" with rating "(.*)"/ do |title, rating|
  Given %Q{I am on the Create New Movie page}
  When  %Q{I fill in "Title" with "#{title}"}
  And   %Q{I select "#{rating}" from "Rating"}
  And   %Q{I press "Save Changes"}
end

Then /I should see "(.*)" before "(.*)" on (.*)/ do |string1, string2, path|
  Given %Q{I am on #{path}}
  regexp = /#{string1}.*#{string2}/m #  /m means match across newlines
  page.body.should =~ regexp
end


