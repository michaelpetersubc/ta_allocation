## Requirements

The algorithm itself was written by [James Yu](https://github.com/jbrightuniverse), a phd student at the Vancouver School of economics.  It is written in the julia language which is popular at the VSE.

The implementation requires access to a webserver to run the admin interface and collect preferences from faculty and student. Since web servers often run with limited resources, a separate computation server may also be required to run the algorithm itself.  Admin access on these servers isn't required unless the algorithm is run on a separate server.  In that case information will have to be transferred between the two servers using an api or an ssh tunnel. 

The admin interface for the algorithm is written in Php as a Drupal 7 module. The module uses the [page load progress module](https://www.drupal.org/project/page_load_progress) to lock the webpage that triggers it when the algorithm is running.  

The system was developed using mysql, though drupal works with many sql databases.

The module makes use of the the standard drupal users database table to hold information about faculty members.  By default drupal doesn't have first and second name fields in a user's account.  Names are looked up using the following small code bit:
```
  public function get_user_name($user_id) {
      $query = "select l.entity_id, l.field_last_name_value as last, f.field_first_name_value as first 
        from field_data_field_last_name l join field_data_field_first_name f on l.entity_id=f.entity_id where l.entity_id=?";
        $result = db_query($query, [$user_id]) -> fetchAll();
        return $result[0] -> first." ".$result[0] -> last;
    }
```

This requires adding a pair of fields to the user's account called `last_name` and `first_name`.  If you have this information in some other format, you can adjust the query accordingly.  You'll find this code in the file called `ta_alloc_namespaced_classes.php` (just do a search to find the function).

The code also assumes that the database that holds the allocation model is defined as an external database in the `databases` array in the drupal settings.php file.

Basic install (this documentation is still very preliminary):

1. Create a symlink to the module directory in whatever folder you use to store your drupal modules.
2. Enable the drupal module
3. Load the `ta_alloc_schema.sql` file into mysql to create all the database tables you'll need.
4. Add an external database to your drupal `settings.php` file to add the new database you created in the last step to drupal.

You will then have access to the administrative interface and can start building a simple implementation.

Bugs are likely.  The current implementation works locally, but locally is always a very special set of circumstances that are not well understood.

## Logic

At a high level, ta allocation is a many to one matching problem.  For example, the Econ101 course in micro has 10 different instructors teaching it.  Each instructor has 3 teaching assistants.  

The way the software here handles this is to convert the problem into a one to one matching.  Maybe the best analogy is to think of two sides of a market.  One side are the demanders, the other side are suppliers.

The demand side is designed to be a collection of abstract allocation <em>blocks</em>.  Each block can be assigned a collection of properties, for example, a course number, a lecture number, the identity of the instructor and any discussion groups associated with the block.  A block then defines a possible assignment that a student be allocated. 

The supply side is simpler.  There is again a collection of allocation blocks.  Each student who is supposed to be a paid teaching assistant during a session is put into one of the student allocation blocks.  The matching problem then becomes a one to one matching problem in which each block from the supply side is assigned to a single allocation block on the demand side or is left unmatched.

Part of this design choice is motivated by the idea that allocation blocks are abstract.  The algorithm just needs to know id numbers and random scores for all these blocks.  It then processes a pair of partial orders representing preferences of each block for the blocks that define the other side of the market.

As the allocation blocks are abstract, the algorithm will work for a broad variety of different environments.  This breaks the whole process up into three parts:  the algorithm, the data model and the admin interface.

The data model describes the environment in which the algorithm is supposed to operate. The one implemented here is given in the sql directory of the code.  Each mysql table represents a set of potential properties of an allocation block.

For the ta allocation problem, there is obviously a set of courses and a set of students.  Allocations are computed for different sessions - for example, Fall 2022 and Spring 2022 are likely different sessions.  Courses have different lectures with different faculty members teaching different lectures.  Lectures may have discussion groups that teaching assistants have to lead.

The idea is to create a new allocation block than attach a bunch of these properties to it.  So a single allocation block might have an attached course, lecture, session, instructor and discussion group. 

In the implementation here, a course allocation block has a lecture_id and a required ta type (Phd, or Masters) attached to it, along with its random score.  The lecture itself defines the course, instructor, session.  Discussion groups are created separately with attached course id numbers.

A student allocation block has a student id and the degree the student is working on, either Phd or Ma.

The final bit of the construction is the admin interface.  It is used to create and edit courses, students, lectures, etc. The one provided here is written in php with drupal 7 as the assumed website framework.



## Preferences

The system allows students to view each of the demand side allocation blocks or assignments.  It allows them, but doesn't require them, to rank as many (or as few) of these as they want.  They do this is two parts. In the first part they select a subset of assignments they are particulary interest in.  By default, selecting an assignment for ranking is interpreted to mean that the student strictly prefers any  assignment she has selected to any assignment she hasn't selected.  The assignments that aren't select are assumed to be equally acceptable - she is willing to do any of them but doesn't care which one.

At that point, part 2 allows the student, if she wants to, to provide a partial ranking over the assignments she has selected. She can also, at this point, choose assignments she is not willing to do.  This is accomplished by assigning weights to each assignment with -1 indicating that a student is unwilling to accepts an assignment.

Assigning a weight of -1 is interpreted to mean that the student would rather have no assignment at all.  An implication is that the student could end up unmatched. (For example, choosing all the assignments in step 1 then giving them all a weight of -1 except for the assignment everyone wants will result in the student losing their ta job). 

At the other extreme a student could select all allocations and assign them weights that support a complete order over all allocations.  Weights don't have to be unique, which permits partial orders.

There are quite a few choices, 75 in the fall session at UBC, so ranking all the possible choices would probably not be worth it.  So the system anticipates a lot of indifference.

The process is indentical for faculty expressing preference over potential students.  A faculty member teaching a course that has multiple teaching assistants assigned to it must rank students for each of their assignments independently.

To sum up, each demand side block has partial preferences over all the supply side blocks and conversely.

The base version of this system, there is no functionality at all provided to supply information about the various allocations.  This would seem to be idiosyncratic.  Any additional functionality could be added to the drupal module.

## Algorithm

As mentioned, the algorithm is written in the julia programming language.  The way the algorithm fetches data is by using a rest api.  The code for this api is included in this package.  The code currently uses http basic authentication to protect the api.  This has to be configured separately.  This was designed so that the algorithm could be run on a different server than the webserver.

Unfortunately, the current version of the algorithm writes output of the algorithm directly back to a mysql database, which is inconsistent with the api approach that fetches the data. TBD.  The output of the algorithm is very simple, a collection of matched pairs along with a session id all written in json, so this shouldn't be hard to customize.

