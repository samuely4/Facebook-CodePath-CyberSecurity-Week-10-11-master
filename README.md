# Week-10-11
> Time taken to complete the assignment in total: 24 hrs.
* Summary: In this assignment, we setup a basic honeypot and demonstrate its effectiveness at detecting and/or collecting data about an attack. Utilizing the opensource Honeynet Management Platform - Modern Honey Network (MHN) - The following basic principles were demonstrated:

    * Successful configuration and deployment of a network-accessible honeypot server with two primary features:
      * An attack surface that is vulnerable or exposed in some way to network-based attacks
      * A network security feature such as an IDS (Intrusion Detection System) configured to detect and log such attacks
    * Illustration of at least one attack against the honeypot that can be detected or logged in a way that captures information about        the attack or the attacker

 ## Assignment Report:
 1. Sensors deployed: p0f, dionaea, snort
 * The following GIF contains a walkthrough that verifies the creation of the MHN-server and the deployment of the three aforementioned sensors
 * Two GCP instances were created. The first is callend mhn-admin to run the MHN-server. The second is called mhn-honeypot-1 which holds the three sensors: p0f, dionaea, snort. To verify that the configuration of the MHN-server and the sensors are set up correctly, we run the command ```sudo supervisorctl status```
 * [session.json file can be found here](https://github.com/Samuel665/Week-10-11/blob/master/session.json "linkjson")
 * Number of attacks caught so far for each sensor:
   * dionaea: 4921
   * P0f: 15686
   * snort: 2390
- [x] GIF Walkthrough
  ![picture alt](https://github.com/Samuel665/Week-10-11/blob/master/week1011.gif "assingment10&11")
       
       
   
- [x] The following GIF contains a walkthrough that illustrates the TOP 5 Attacker IPs, TOP 5 Attacked ports, TOP 5 Honey Pots, and TOP 5 Attack Signatures 
  ![picture alt](https://github.com/Samuel665/Week-10-11/blob/master/top5week1011.gif "assignment10&112")
 
 2. Description of challenges encountered during this assignment
* The script to set up MHN-server takes a long time to finish its executoin cycle. For several times the SSH session timed out and I had to rerun the installation script again which consumed a lot of time. I found out that there are serveral terminal multiplexers like TMUX or Screen which actually allow you the run any process on the GCP instance without having to have any active session throughout the lifcylce of the process execution.

* Once I had the MHN-server set up and running. I attempted to do ```sudo supervisorctl status``` for the first time to actually find out that celery worker process is not running and it has a fatal error: ```(fatal exited too quickly (process log may have details)```. The fix to that error was to actually look at ```celery_worker.err``` log and trace the error. I found out that the error was caused by a version mismatch of one of Python dependencies (which was redis-py) in ```env``` directory in ```/opt/mhn/``` folder. So, I had to update to a newer version of redis-py to be able to run the celery worker process. After updating the mismatched dependency, I had to run two commands: ```sudo supervisorctl stop all``` and ```sudo supervisorctl start all``` which restarted all processes including the celery worker successfully.

* For some reason after setting up the sensors on ```mhn-honeypot-1``` instance, I was not logging any attacks. I checked the rules I had to create and they were set up correctly. However, I additionally had to follow some other steps which can be found on the this website https://hackmd.io/s/SkWWWHa0W. The steps are the following:
   * to allow traffic on certain ports edit the allow-http-traffic firewall rule by going to the menu
   * scroll to vpc network and click firewall rules
   * click default-allow-http
   * click edit and change protocols and port to Allow all
   * click save
 
 # Resources Used in Lab/Assignment:
 * Goolge Cloud Platform
 * https://github.com/threatstream/mhn
 * http://bagl.io/sec/2015/09/05/MHN-_Deploying_a_Honeypot_Sensor
 * https://hackmd.io/s/SkWWWHa0W
 # Note
 * Materials used to do this lab are to be found in the repository
 
 
 
