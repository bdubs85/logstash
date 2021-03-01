# Logstash
Skills test repository for Logstash files


## The Requirements
The requirements for this skills test are:
1) Provide the Logstash configuration on a public Git repo.
2) Write a short description of how data is ingested and output from Logstash based on my solution.


## VM configuration and initial setup
I created a cloud VM running Ubuntu 20.04 for this exercise. After spinning up the VM, I updated the system using apt-get and then installed Logstash.
The instructions to install Logstash can be found at: https://www.elastic.co/guide/en/logstash/current/installing-logstash.html.

## Changing the configuration
After installing Logstash, I created a directory and file to store the sample Security Log Data:
```
mkdir -p /home/cloud_user/grok
nano /home/cloud_user/grok/sample.log
```
I then pasted in the data as provided in the instructions and saved the file.

From here, I could have edited the regular logstash configuration file, but I instead created a new configuration file for this example:
```
sudo nano /etc/logstash/conf.d/logstash_demo.conf
```

**Contents of the configuration file:** [Configuration](https://github.com/bdubs85/logstash/blob/main/logstash_demo.conf)

This configuration uses a *sample.log* file input to logstash. 

From here, I configured a Grok filter to match the various segments of the log and map them to a useful semantic. 
Notice that the severity on the input file and the required output are dissimilar: *"1"* on the input log and *"High"* on the output requirement. 
To address this requirement, I used ```severity="%{INT:severity_lvl}``` 
to input the *"1"* as a temporary value and then used a transform filter to dictionary match that input to a desired output of *"True"* and then removes the temporary field. This way, you can create different dictionary values to transform other severity levels dynamically.

Finally, I have Logstash output to both a file and stdout for console viewing. One thing to note is I did have to change ownership permissions of the /home directory using ```chmod o+x /home``` in order for Logstash to be able to write the *output.json* to that file/directory. Also, I did add the *"overwrite"* write behavior to truncate the file and only show the output from the last run.
## Running Logstash
After creating the separate configuration file, I then ran logstash, using the "-f" flag to use my configuration file when running.
```
sudo /usr/share/logstash/bin/logstash -f /etc/logstash/conf.d/logstash_demo.conf
```
Logstash then runs, outputting to both the [output.json file](https://github.com/bdubs85/logstash/blob/main/output.json) as well as [console output](https://github.com/bdubs85/logstash/blob/main/std_output):
```
           "severity" => "High",
            "message" => "<14>1 2016-12-25T09:03:52.754646-06:00 contosohost1 antivirus 2496 - - alertname=\"Virus Found\" computername=\"contosopc42\" computerip=\"216.58.194.142\" severity=\"1\"",
     "syslog5424_app" => "antivirus",
               "path" => "/home/cloud_user/grok/sample.log",
     "syslog5424_pri" => "14",
          "source_ip" => "216.58.194.142",
           "@version" => "1",
               "host" => "05f7dbdb731c.mylabserver.com",
      "syslog5424_ts" => "2016-12-25T09:03:52.754646-06:00",
           "hostname" => "contosopc42",
        "description" => "Virus Found",
         "@timestamp" => 2021-03-01T18:47:52.185Z,
    "syslog5424_proc" => "2496",
    "syslog5424_host" => "contosohost1",
     "syslog5424_ver" => "1"
```
All of the required data (but not exclusively) for this skills challenge is contained in these outputs.