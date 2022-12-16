# Configuration

## Requirements

The administrative interace to the database system that drives the allocation is written for drupal 7 - time pressure during development.  If you aren't familiar with drupal, the current version of drupal is drupal 10.  Drupal 10 is fine, but it is nothing at all like drupal 7.  Basically, it is a completely different framework.  Updating the software here for drupal 10 would require a major amount of work. It would be about the same challenge as rewriting the interface here for laravel, or wordpress.   At the moment, the life of drupal 7 is extended every year.

Since it is drupal, it is written in php.  Database is mariadb (or mysql) and some features are mysql specific.

The algorithm itself is written in julia.  It current version uses a rest api (which must be configured) to feed it data.

## Required Configuration

There is a basic configuration form at `https://yourwebsite.ca/ta_alloc/config`

