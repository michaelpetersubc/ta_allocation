# Session Management

The very first step in configuration is to create sessions, one for each term, or quarter.  

The admin page opens when you click the top menu link which points to [https://yourwebsite.ca/ta_alloc](https://yourwebsite.ca/ta_alloc). The admin page looks like this:

 ![alt](documentation/user_manual/assets/session.png "Session")
 
Three sessions have already been created in this illustration - one for spring and fall, and a fake session that will be used mostly for this documentation.

 If you click the create session button, for example to set a summer session, you'll get a new form to fill out.  There are four fields to be filled out in the new form.  At the moment, only the first two are important. They are, the name of the session and a short note describing what it is for.  Don't worry about the name or your  notes, you can change them later by using the edit session link.

 ## Which session to use

 Once you have created your sessions, you can also use this list to choose which session to `work` on.  Whenever you are editing data, for example adding allocations or changing course details, your changes will apply to the session that you choose here.  Notice that the title tells you which session you are working on.
 
 This setting is unique to you.  Anyone else who is working on the ta data must choose their own session to work on.  For example, if you are working on session 2 and they do not choose a session, they will end up working on session 1 (the default).  
This is something to remember when you find unexpected things happening, and realize it is possible you are working on the wrong data.


 ## The public session
The session you happen to be working on can be any session you choose.  At any point in time, however, there will also be an active public session.  This session determines what students and faculty see when they change their preferences.  The public session is also the one connected to the algorithm.  

If you make changes, say by adding a course allocation, to session 1 and the public session is sessino 2, then your changes can have no impact on the allocation you get if you run the algorithm.

  The public session is determined by the second section on the `ta_alloc` main page with section title `The algorithm is using session `, as below.

  ![alt](documentation/user_manual/assets/alogorithm_choice.png "Algorithm")
  
  According to this figure, the algorithm is using data from session 1. Notice the line that says `The algorithm is currently unlocked`.   When the algorithm is unlocked, you can run it.  When the algorithm is locked, the algorithm won't run.  This prevents you from inadvertently over writing the allocation of tas to courses that you have already made public.
  
  The algorithm lock is set and unset by using the `Edit session .` button described above and changing the setting in the form.  Make sure the algorithm is locked once you have chosen a final allocation.  You an unlock it again when the same session comes around next time.