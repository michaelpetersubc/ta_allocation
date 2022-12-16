# Ta allocation algorithm

This algorithm uses the well known Deferred Acceptance Algorithm (aka the Gale Shapley algorithm) to give teaching assistants their assignments. This algorithm relies entirely on the preferences of the students and professors involved to find a good allocation. 

The software included uses the 'student proposing' version of the algorithm.  When students and professors fully articulate their preferences, the algorithm create a stable matching of students to assignments.  What this means is that whenever a student would prefer an alternative assignment to the one provided by the algorithm, the algorithm will ensure that assignment has been given to a student who is preferred by the professor in charge of the assignment.

Furthermore, there is no way for students to benificially influence their final assignment by manipulating the preferences they submit.

To read more about the algorithm and all it uses you can use [this nice description from Al Roth](https://web.stanford.edu/~alroth/papers/GaleandShapley.revised.IJGT.pdf) to learn about the many other applications where this algorithm has been used.

## Gathering preferences

The desirable properties of the assignment that the algorithm generates rely on the assumption that the way a professor evaluates two different students for an assignment does not depend on what other teaching assistants they are assigned. In large courses, where many teaching assistants work on the same course, this may not be true.  The software provided here assumes this issue away. 

The number of alternatives that students and professors have to consider can be large. So the preferences that are available in practice may reflect a lot of 'indifference' (reflecting mostly a lack of information available about these alternatives).  Nothing in the software here will help with that.

In cases of indifference, allocations are made randomly here. So if no one expresses any preferences at all, students will be randomly allocated to the various assignments and you'll get a workable outcome without any effort beyond providing information about what the alternatives are.

## Brief Decription

Once participants have expressed the preferences over the alternatives, the algorithm starts mechanically making proposals by students to professors.  A proposal by a student is always made to the whatever assignment they most prefer among those assignments that haven't already rejected their proposal. If a professor receives a proposal for an assignment that hasn't been filled, they temporarily accept the proposal and wait to see if there are any other proposals.  Whenever they receive a proposal by a student they prefer to the incumbent, they reject the incumbent (who can then make a propsal to their next best assignment) and accept the newcomer.  This continues until all the students who haven't been accepted for any assigments have been rejected by all of them.

Once all is said and done and a student has received his or her assignment, they may wish they had done better.  However they can be assured that if they revealed to the algorithm that they preferred some other assignment, the algorithm would have made a proposal on their behalf for that assignment.  Since the student didn't get it, they know it was rejected because the professor in charge of the assignment was assigned someone they preferred.

## The pitfalls of indifference

One potential issue is the meaning of 'preferred'.  In the sentence above, a professor who rejects an application in favor of someone they prefer, may actually be indifferent.  The other applicant may be preferred only because they won a lottery. Here is a simple example to illustrate.

<table>
  <tr>
    <td>  </td> 
    <td><table><tr><td>A</td><td>B</td></tr></table> </td>
  </tr>
  <tr>
    <td><table><tr><td>1</td></tr><tr><td>2</td></tr></table>  </td> 
    <td>
      <table>
        <tr>
          <td> .3,1</td>
          <td> .7, .8 </td>
        </tr>
        <tr>
         <td> .1,.2</td>
         <td> .5, .5 </td>
        </tr>
      </table>
    </td>
  </tr>
</table>

In this table, the rows correspond to assignments, the columns students.  Each cell contains a number that represents a preference.  An integer value represents a real preference, a fractional value is just a random score.

The first element in each cell represents the assignment preferences, the second the student preferences.  So neither assignment cares which student they get.  Student B doesn't care which assignment they get, but student A strictly prefers assignment 1 to assignment 2.

Since  assignment 1 randomly assigns a higher score to B, while B randomly assigns a higher score to assignment 1, deferred acceptance algorithm will have 1 reject A's application to 1 in favor of B who also applies.  As a result A gets assignment 2 while B gets assignment 1.

This isn't pareto optimal because A and B could switch assignments.  A would be strictly better off while no one else would care.

A question is whether this could be resolved by running a top cycle algorithm where each student and professor points to their weakly preferred assignments.  If it did, it would probably be impractical anyway.  Student A would be willing to take the effort to point to assignment 1 in the example above, but since the others are indifferent, they wouldn't bother, and no cycle could be found.

## Things this approach can't do

There are many limitations to the Gale Shapley approach.  The problem with indifference was explained above.

A potential pitfall is that it is left entirely up to instructors to choose the best teaching assistant for the course they teach.  In many cases this won't work well.  Instructors may not know much about the options.  For example, large first year courses tend to use masters students as teaching assistants.  The masters students who are available as assistants change every year - no one but their instructors knows much about them.

There is also a difference between knowledge and belief.  Many instructors have strong views on the characteristics their assistants need even though these characteristics don't fit their course.  As was pointed out to me by someone who has done a lot of research into discrimination, preferences may well be a reflection of unconscious biases or outright prejudice.  Leaving it up participants to choose freely may lead to undesirable outcomes.

Centralizing andalgorithm and restricting choices may help when lack of knowledge is a problem.  It is less likely to improve matters if the problem is bias.  If nothing else, the current algorithm can be used without any preferences at all (except courses can specify Phd students only) and it will generate a feasible random assignment.  

Finally, the gale-shaply agorithm doesn't work for fractional assigments. The algorithm and implementation provided here were partly inspired by [Marcin Peski](https://www.economics.utoronto.ca/index.php/index/person/person/faculty/1318) who developed a more centralized ta allocation algorithm, partly to deal with fractional assignment.  His approach is 'planner knows best'.  He doesn't make his implementation available, but he helped us considerably to set up this version.

