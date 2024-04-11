
## Crowdstrike Falcon
### Endpoint Security 
### what is falcon?
- Crowdstrike Falcon is a cloud based, next generation endpoint protection solution.
- Falcon provides software as a service.
- it provides advanced detection, prevention, monitoring and search capabilities, allowing analysts to defend against sophisticated threats and adversaries.
- It offers remote visibility across endpoints throughout an environment.
- It gives very detail info about the attack like who,what,when,why,where and how the attack.
- ### How does Falcon works?
- Falcons uses two things to make its work done
==1. Sensor
2.Endpoint cloud portal==

- falcon has billions of sensors deployed over 80billion endpoint events in over 176 countries.
- falcon uses the sensors to fetch the info from the endpoints or the device in which its installed and send it to cloud portal for the analysis and other monitoring tasks.
- Falcon sensor is a lightweight sensor which is deployed into the endpoint without affecting the endpoint performance.
- It gathers the info from the endpoints and takes proactive detection and preventions actions.- The sensors detects and defends the attacks occurring on the disks.
- The platform continuously watches for suspicious processes, events, and activities, wherever they reside
- form the platform we can do advance protection like allowing and blocking sites customly just like nsgs in azure vm.
### WHat happens to the data send to the falcon endpoint detction portal?
- the data extracted from the device is send to the portal by sensors.
- then this data is analysed and will draw some link between the events triggered and the data.
- Here falcon uses Crowdstrike threat graph data model., whoch will analyzes and detects the new threat.

### Capabilities of Falcon

- Cloud based antivirus (Cloud AV)-
  for known threats falcon provides cloud antivirus
- Indicators of attack (IOA) detection-
  for known threats falcon provides cloud antivirus
  for unknown and zero-day threats falcon provides IOA detection(Indicator of attack). IOA uses AI and ML to build models which will predict the never seen before threats and malicious activities with high accuracy .
- Crowdstrike's Threat Graph data model-
  this generates graph based patterns which helps the predictive models of IOA to get insights and be more accurate.

### Falcon Sensors?

  - SO sensors are basically agents which get deployed int the host or the endpoints of which the monitoring and threat dtection has to be done .
  - It takes a small space and less time to get deployed and also it wont affect the performance of the host.
  - this sensor will send the data of detection and threat events of the host to the SIEM.
  - It also monitotrs and anlayzes the data and events through web portal i.e Falcon endpoint detection threat intelligence portal
  - the sensors dont require any controllers to install and maintainance, no on-premises equipment.
  - Therefore Falcon is 100% cloud based solution, which offers security as a service.


### What falcon sensor sends to the Cloud portal?
- it  is designed to maximize visibility into real-time and historical endpoint security events
- it gathers the event data which is necessary to identify , understand and respond to attacks
- when any threat is detected from the data sent then it requests for more info to get full insights of the suspicious activity spotted.


### how does crowdstrike secures the customers data ?

- It basically usus TLS encryption for sneding data between the sensor and cloud
- Crowdstrike uses CERTIFICATE PINNING on the sensor side. This means that the sensor will only communicate to the cloud endpoints that have a known certificate. 
- It suggests to have only one AV in the host, because two different AV will have different configurations, which will lead to conflicts and may be fluctuations in the data.
- provides you the ability to allow our cloud endpoints in your firewalls to ensure that your Falcon sensors only communicate with CrowdStrike.
- crowdstrike provides every customer a unique ID and all the event and data shared to the portal through the sensors will have that unique Id , now this wont let the data and event info of the host to be leaked or robbed.
- crowdstrike works on industrial standards AES256 encryption. which says every data including backups should be encrypted.
- crowdstrike wont allow each employee to have accesss to all the data recieved by the portal through sensor, hence it is doen accroding to the business need.
- also takes care of the multi factor authentication


  



