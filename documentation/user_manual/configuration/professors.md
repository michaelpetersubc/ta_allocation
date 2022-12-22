# Configuring Professors and Instructors

##  Make sure faculty have names

The database here assumes that all the instructors are registered users in your drupal installation.  It uses drupal user ids extensively.  (Students are not assumed to be part of your database since they are constantly changing.)
Unfortunately, drupal 7 does not configure first and second names for users by default.  To fix this use the Configuration menu in drupal, then choose Account Settings, then Manage Fields. Add two fields by entering `first_name`, then  `last_name` by using the `Add new field` option.  This will ensure that the faculty member's name is correctly displayed when tas are looking at their assignments.

## Install the UUID module for drupal 7

After installing this module, use its configuration options to ensure that all users in your drupal installation have uuids if they don't already.  These are used to allow login free access to preferences using what are essentially tracking urls in email messages.  

You don't need to worry about assigning uuids to students as this is done within the module code.

## Add drupal role names

The module needs to know which of your drupal users should be included in list of instructors.  This is done using drupal roles.  You may well have already assigned you faculty to roles, for example, you might have a role called `Full Professor` which is assigned to all of your full professors.  You can find out by clicking the `People` link on your admin toolbar, the following the `Permissions` link on the upper right, then the `Roles` link. 

If you haven't done this already, you can create the role names you want here.  You'll need to go through your various user accounts and assign them to these new roles.  Then make sure you go back to the [configuration page](configuration.md) and add the role(s) you have created (comma separated).

