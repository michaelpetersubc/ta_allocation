# Configuration

## Requirements

The administrative interace to the database system that drives the allocation is written for drupal 7 - time pressure during development.  If you aren't familiar with drupal, the current version of drupal is drupal 10.  Drupal 10 is fine, but it is nothing at all like drupal 7.  Basically, it is a completely different framework.  Updating the software here for drupal 10 would require a major amount of work. It would be about the same challenge as rewriting the interface here for laravel, or wordpress.   At the moment, the life of drupal 7 is extended every year.

Since it is drupal, it is written in php.  Database is mariadb (or mysql) and some features are mysql specific.

The algorithm itself is written in julia.  It current version uses a rest api (which must be configured) to feed it data.  The reason for this quirk is that you 
may want people to work on the algorithm in julia without giving them access to your database.  Simple api software that will work is included in the software bundle, but you'll have to configure drupal and apache to use it.

## Required Configuration

There is a basic configuration form at `https://yourwebsite.ca/ta_alloc/config`.

![alt](../assets/config.png "Configuration")

There are four settings on the form.  The first two are julia specific settings and you probably don't have to change them. 

The third setting is a prefix for your courses.  If you set this, then you can use numbers when you specify a course name.  If you set the course name to be 100, then it will render everywhere as Econ100 as it is set above.  If you set a course name with a prefix of its own, it will override this setting.

If you just set all your course names with their own textual prefixes, this setting will have no effect.

The fourth setting is faculty roles.  These are different for every drupal site.  You can see the list of roles on your site (assuming you have adminstrative permissions) by editing any user account and looking at the possible role assignments.  Add a comma separated list of role, with each role written exactly as you see it on the user page.

This is used to assemble the list a drupal accounts that belong to people who should be included in the list of instructors to which lectures can be assigned.

## Drupal Settings

The final setting needed is to tell drupal the name of the database that contains the ta allocation data.  Add a string like the following to your `settings.php` file in the `$databases` array.
```
'ta_allocation' => 
  [
    'default' => 
      [
           'database' => 'ta_allocation',
           'username' => 'username',
           'password' => 'password',
           'host' => 'localhost',
           'port' => '',
           'driver' => 'mysql',
           'prefix' => '',
      ],
  ],

```
Of course, replace the username and password with the strings that are appropriate for your application. 

Finally, you'll need to set up your drupal menu for administration.  Here is the picture of the one we use.
![alt](../assets/menu.png)

Where you put the menu depends on the theme you use, and is up to you.  Here is a list of paths you can use to set up your own menus.  Use the string associated with the item as the path when you create your drupal menu item.

1. `ta_alloc` - this is a link to the main adminstration page as described in [the sessions page](../sessions/create_session.md).  This link allows you to choose a session to work on, set the algorithm session and run the algorithm.
1. `ta_alloc/config` - the link to the configuration page (julia path information, prefex for courses, drupal roles)
1. `ta_alloc/courses_list` - list, create and edit your courses
1. `ta_alloc/student_list` - list, create, edit students, set them active of inactive
1. `ta_alloc/status` - a status report and a link to send email notifications (don't experiment with the email links unless you have only experimental student and faculty entries)
1. `ta_alloc/session` - view, create, edit, and allocate for all the lectures in the session you are working on.  In our menu this is the link in `Course Allocations`
1. `ta_alloc/by_degree/1` - view all the lectures (and their instructors) that have been assigned phd students as tas.   Once you have run the algorithm the list of assigned students will appear here.  This isn't a public page so you can use it as a way to inspect your current allocation.
1. `ta_alloc/by_degree/2` - same as the last item except for lectures that have been assigned masters students
1. `ta_alloc/faculty_list` - a list of faculty who have been assigned tas.  Clicking the `faculty_id` link will let you see the preferences of the corresponding faculty member.
1. `student_allocations` - a list of students and their allocation status.  Clicking the `id` link will show you the corresponding student's preferences.
1. `ta_alloc/student_by_degree/1` - a list of allocations for all the phd students.  Replace the `1` in the path with `2` and you'll get the masters students instead. Once you have run the algorithm, the allocation is show so you can use this link oth to check students preferences, and to see how they are allocated.   The information here is not made public until you lock the algorithm. 