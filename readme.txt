7/1/13  

simulation-scenario-db provides persistense for simulation-scenario through Hibernate 
https://github.com/sjdayday/simulation-scenario-db.git

Libraries:  
  log4j:  logging
  hibernate3:  persistence to hsqldb:  see testDatabase folder for tests
  jta:  compile-time requirement only for MockHibernateTransaction
  google-diff-match-patch:  downloaded google diff class; packaged by me as a jar
  hsqldb.jar:  hsqldb
  dom4j:  hibernate
  slf4j-api: hibernate  
  slf4j-log4j12: hibernate
  commons-collections:  hibernate
  antlr:  hibernate
  javassist:  hibernate
  fitlibrary:  fitlibrary [built by hand ca. 2011.  Fitlibrary was developed by Rick Mugridge:  http://sourceforge.net/apps/mediawiki/fitlibrary/index.php?title=Main_Page]  

The tests have dependencies on HSQLDB
  File / Import / Run/Debug / Launch Configurations; then select from the project root
   hsqldb.launch 
   hsqldb Swing.launch 
   all simulation scenario tests.launch (if you don't already have one)
   all simulation scenario database tests.launch (for the tests in testDatabase)
  run:  hsqldb 
  then: all simulation scenario database tests
 
 
 Log4j: src/log4j.properties currently set to "log4j.rootLogger = debug..."; change to "log4j.rootLogger = error..." to cut back on verboseness   
        


Questions:  stevedoubleday [at] gmail [dot] com

Thanks for your interest!

Steve Doubleday
  