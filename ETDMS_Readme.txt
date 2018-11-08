Title:	Setting up ETD-ms on the DSpace system for LAC
Author:	Jonathan Roby (robyj@cc.umanitoba.ca)
Date:	13th April 2005


Introduction
============
DSpace does not by default allow harvesting of ETD-ms records
by OAI providers, which is a shortcoming in terms of allowing
repositories to spread their records. Here at the University of
Manitoba, we've written our own ETD-ms crosswalk for use in our
DSpace instance (named MSpace).
This document will provide instruction in setting up the ETD-ms
crosswalk to allow the Library of Archives Canada (LAC) to harvest
records from your repository.


Instructions
============
1) Move the University of Manitoba ETD-ms crosswalk (ETDMSCrosswalk.java)
   to your <dspace source>/src/org/dspace/app/oai/ directory.

2) Edit the crosswalk as applicable to your site.

3) Rebuild the site by using a command line similar to:
   /usr/local/ant/bin/ant -D ./config/dspace.cfg build_wars.

4) Copy the dspace.war and dspace-oai.war to your <tomcat>/webapps/dspace/
   directory.

5) Copy the <dspace source>/build/classes/org/dspace/app/oai/ETDMSCrosswalk.class
   to <tomcat>/webapps/dspace-oai/WEB-INF/classes/org/dspace/app/oai/ETDMSCrosswalk.class

6) Edit the <dspace>/config/templates/oaicat.properties file to add the following:
   (Note: this is the <dspace> directory, not the <dspace source> directory!)

   in the "# Custom Identify response values" section of the file, add the following line:

   Identify.description.0=<description><oai-identifier xmlns="http://www.openarchives.org/OAI/2.0/oai-identifier" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.openarchives.org/OAI/2.0/oai-identifier http://www.openarchives.org/OAI/2.0/oai-identifier.xsd"><scheme>oai</scheme><repositoryIdentifier>mspace.lib.umanitoba.ca</repositoryIdentifier><delimiter>:</delimiter><sampleIdentifier>oai:mspace.lib.umanitoba.ca:1993/94</sampleIdentifier></oai-identifier></description>

   ...and modify the identifiers to suit your site.

7) also in the oaicat.properties file, at the bottom of the file theres a "Crosswalks.oai_dc=org.dspace.app.oai.OAIDCCrosswalk" line. below this, add this line:
   Crosswalks.oai_etdms=org.dspace.app.oai.ETDMSCrosswalk

8) run the command <dspace>/bin/install-configs to setup the new oaicat.properties file

9) To test your new crosswalk, I used to use the OAI repository explorer at U of Virginia, but that seems to have
   disappeared, so I use a mirror of that site at http://re.cs.uct.ac.za/. Some of the fields may or may not be
   straight forward. The base URL is your site URL + "dspace-oai/request" 
   e.g. http://mspace.lib.umanitoba.ca/dspace-oai/request
   And the Identify response should work nicely as a first test. To see your records, I usually set the 
   "from" date to 1999-12-31 and "to" date to "2005-12-31" and metadataPrefix to oai_etdms. Clicking on
   the "List Records" link to the left should display your records.
