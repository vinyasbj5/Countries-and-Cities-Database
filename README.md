# Countries & Cities Database

This library will help to build a database containing continents, countries, cities, and currencies.

Due to copyright reasons, I can't provide the final compiled database in this repo.

However, you can easily clone this repo and follow the instructions below to create it yourself.

## Setup

* Clone repo and run `bundle` to install required rubygems

* Download the [GeoLite2 Countries/Cities](http://dev.maxmind.com/geoip/geoip2/geolite2/) CSV files

  http://geolite.maxmind.com/download/geoip/database/GeoLite2-Country-CSV.zip
  http://geolite.maxmind.com/download/geoip/database/GeoLite2-City-CSV.zip

* Download the [GeoLite Cities](http://dev.maxmind.com/geoip/legacy/geolite/) CSV file

  http://geolite.maxmind.com/download/geoip/database/GeoLiteCity_CSV/GeoLiteCity-latest.zip

* Unzip and move the downloaded folders into /vendor folder

* Edit `parse.rb` and adjust the config settings at the top

* Setup database

  $ rake db:migrate

* Start parsing through all the data to create the database

  $ ruby parse.rb

## Usage

    > load './app.rb'
     => true
    > Continent.count
     => 7
    > Country.count
     => 247
    > City.count
     => 71475
    > continent = Continent.find_by_code("NA")
     => #<Continent id: 6, name: "North America", code: "NA", translations: nil>
    > country = continent.countries.find_by_alpha2("US")
     => #<Country id: 240, continent_id: 6, currency_id: 59, name: "United States", alpha2: "US", alpha3: "USA", country_code: "1", world_region: "AMER", region: "Americas", subregion: "Northern America", latitude: 38.0, longitude: 97.0, translations: {"en"=>"United States", "es"=>"Estados Unidos", ...}>
    > country.currency
     => #<Currency id: 59, name: "Dollars", code: "USD", symbol: "$">
    > country.cities.find_by_name("New York")
     => #<City id: 63585, country_id: 240, name: "New York", latitude: 40.7528, longitude: -73.9725, time_zone: "America/New_York", translations: nil>

## Export to CSV

Often you'll want to have it in a format that you can easily parse into your own application.

You can export the countries and cities into a CSV file by running the following command:

    $ ruby export.rb

    $ ls *.csv
    cities.csv countries.csv

    $ cat countries.csv
    Name,Alpha2,Alpha3,Continent,World Region,Region,Subregion,Latitude,Longitude,Currency Code,Currency Symbol
    ...
    United States,US,USA,North America,AMER,Americas,Northern America,38.0,97.0,USD,$
    ...

    $ cat cities.csv
    Name,Country,Latitude,Longitude,Time Zone
    ...
    New York,United States,40.7528,-73.9725,America/New_York
    ...

## Credits

This library makes use of the GeoLite and GeoLite2 country/cities data created by MaxMind (http://www.maxmind.com).

This library also makes use of the 'countries' rubygem for additional country related information, such as alpha3 code, country code, region/subregion, and translated country names (https://github.com/hexorx/countries).
