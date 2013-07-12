7/5/13  

simulation-scenario-fit provides support for integration tests of simulations through Fitlibrary, developed by Rick Mugridge, on top of Fitnesse and Fit:
  http://sourceforge.net/apps/mediawiki/fitlibrary/index.php?title=Main_Page 
 
Ward Cunningham and Rick Mugridge wrote an excellent book on fit and fitlibrary, "Fit for developing software":
http://www.amazon.com/Fit-Developing-Software-Framework-Integrated/dp/0321269349/ref=sr_1_1?ie=UTF8&qid=1373656067&sr=8-1&keywords=fit+for+developing+software 

Rick has continued to develop fitlibrary.  

Fitnesse was developed by "Uncle Bob" Martin and others at fitnesse.org
Fit was developed by Ward Cunningham, at fit.c2.com
----------- 
 
This project has two parts:  
1) test classes ("fixtures") used in testing simulation scenarios.  This initial version includes exactly one fixture:  ParameterSpaceFixture; others are pending.
2) an installed version of fitnesse and fitlibrary, ready for immediate use to run acceptance tests against the fixtures in this project, or against fixtures in any other project, 
e.g, protection-simulation-fit.   

Part 1

This is a small maven project.  The important dependencies in pom.xml are simulation-scenario (the core project for running simulations), and fitlibrary.  
But fitlibrary is not mavenized; to get this to work, copy fitlibrary-2.0.jar from fitnesse-201104 to directory {your maven repo}/fitlibrary/fitlibrary/2.0/

Since fitlibrary is not in the central Maven repo, and all that is being added to your local Maven repo is the jar itself, without a POM, expect warnings for any Maven operations, like these:    

"Downloading: http://repo.maven.apache.org/maven2/fitlibrary/fitlibrary/2.0/fitlibrary-2.0.pom
[WARNING] The POM for fitlibrary:fitlibrary:jar:2.0 is missing, no dependency information available"

Aside from that problem, you should be able to do a Maven package (don't be alarmed that there are no unit tests) and Maven install to create simulation-scenario-fit-1.0-SNAPSHOT.jar in your Maven repo. 
This jar will be a dependency for any projects that use simulation scenario to do acceptance tests, e.g., protection-simulation-fit. 


Part 2

This project depends on a non-mavenized project, fitlibrary, which in turn depends on an old, non-mavenized version of fitnesse, packaged as fitnesse.jar, available here:
  http://fitnesse.org/fitnesse.jar?responder=releaseDownload&release=20110104
Fitlibrary is downloaded from here, as FitLibrary-2.0.zip:  http://sourceforge.net/projects/fitlibrary/files/latest/download?source=files  (note that this downloads the latest version; if it's not 2.0, the install process may be different.)

I've merged these two into a working fitnesse/fitlibrary installation under directory fitnesse-20110104 in the project root.  

Running the Acceptance tests:

1) You'll need to set two environment variables, one in your environment profile, and the other as a fitnesse command-line argument: 
1.1) M2REPO is a pointer to your local Maven repository, for example:  export M2REPO=/Users/stevedoubleday/.m2/repository
1.2) SIMTARGET is the path to the target directory in this project (simulation-scenario-fit).  
2) Navigate to fitnesse-20110104 under the project root. 
3) Start fitnesse on port 8081, setting SIMTARGET in the -D option, and changing the path after the -d option to point to the src/test/resources directory in this project:  
java -DSIMTARGET=/Users/stevedoubleday/git/simulation-scenario-fit/target -jar fitnesse.jar -p 8081 -d /Users/stevedoubleday/git/simulation-scenario-fit/src/test/resources
4) In a browser, navigate to fitnesse:  http://localhost:8081
5) You should see the fitnesse FrontPage; click on the link towards the bottom:  SimulationScenarioFitAcceptanceTests
5.1)  This page explains the class path requirements in more detail, and additional fitness and java startup options.  Review them for future use.  
6) Click on "Parameter Space"
7) On the Parameter Space page, click on Test in the left Nav.  8 tests should pass. 

The nice shutdown for fitnesse is: http://localhost:8081/?responder=shutdown

Once fitnesse / fitlibrary is installed, it can be used to create and run acceptance tests for any arbitrary project, by starting it with the -d parameter, pointing to the directory where tests should go.  I suggest that the appropriate place for the acceptance tests is: {project_base}/src/test/resources/

------------------

Fitnesse and fitlibrary help:

The fitnesse and fitlibrary code bases forked some years ago.  Both code bases have older versions of pages or code from the other.  It can be confusing keeping them straight.  
Fitnesse help is available from any fitnesse page.  
Fitlibrary help has a tendency to get overridden during the fitnesse install process.  The vanilla installation in fitnesse-20110104 has been modified:  I added a pointer
to fitlibrary help on the front page.  I suggest you start a version of fitnesse just for purposes of reading this help.  

From fitnesse-20110104: 
java -jar fitnesse.jar -p 8082 -o

Both help systems suggest you create and modify pages to play with and learn the system.  Please don't check those modifications back into git.   

----------------

Making it better: 

All of this is pretty manual: "the dumbest thing that could possibly work."

Here are some things that occur to me to make it less painful: 
1) Mavenize fitnesse:  newer versions are mavenized, but the old version integrated here has not been. 
2) Mavenize fitlibrary:  fitlibrary as a lot of jars in its lib path; because it's not mavenized, there is an ongoing exposure to conflicts with jars in any maven project we run at the same time as fitlibrary.
I don't know how hard it would be to test / modify fitlibrary to run against a more current level of fitnesse - a question for Rick Mugridge (http://www.rimuresearch.com/)
3) Get Maven to run the acceptance tests.  There is a Maven plugin to run fitnesse tests.  I'm betting that won't work for fitlibrary tests; FolderRunner is Rick's mechanism for running tests in batch.
     
These are all well beyond my level of competence.  Help gratefully accepted. 

----------------

How it was done:  installing fitnesse and fitlibrary

The steps of the merge of fitlibrary into fitnesse were as follows (this need not be re-done unless you're curious or are trying to migrate to more current versions of either fitnesse or fitlibrary -- see Making it Better):  
Repeating:  Don't do the following steps--they were already done!  
1.  Download fitnesse.jar into directory: fitnesse-20110104 
2.  From that directory, let fitnesse configure itself, using a convenient port (8081, in these examples):  java -jar fitnesse.jar -p 8081
2.5. Optionally, get into fitnesse and run the acceptance tests to make sure all is working:   http://localhost:8081  É. click on Acceptance Tests É click on the Suite button on the left. 
2.6  Shut down fitnesse:  http://localhost:8081/?responder=shutdown
3.  Note that fitnesse creates a directory under fitnesse-20110104:  FitnesseRoot
4.  Download fitlibrary zip file: FitLibrary-2.0.zip (stored in project root, expanded elsewhere as a temp directory).  Note that fitlibrary creates a fitnesse directory.  Copy the contents of that directory to the fitnesse directory:   fitnesse-20110104.  It will both add files and replace files, including FitnesseRoot.   
5.  Start fitnesse from directory fitnesse-20110104, using the command from 2.5.  This will prime the FitnesseRoot directory for its first use.  After first use, adding the -o parameter will speed startup.  
6.  Once fitnesse / fitlibrary is installed, it can be used to create acceptance tests for any arbitrary project, by starting it with the -d parameter, pointing to the directory where tests should go.  I suggest that the appropriate place for the acceptance tests is: {project_base}/src/test/resources/  

---------------

Questions:  stevedoubleday [at] gmail [dot] com

Thanks for your interest!

Steve Doubleday
  