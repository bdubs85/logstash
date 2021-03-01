# Logstash
Skills test repository for Logstash files


## The Requirements
The requirements for this skills test are:
1) Provide the Logstash configuration on a public Git repo.
2) Write a short description of how data is ingested and output from Logstash based on my solution.


## VM configuration and initial setup
I created a cloud VM running Ubuntu 20.04 for this exercise. After spinning up the VM, I updated the system using apt-get and then installed Elasticsearch and Logstash.
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

Contents of the configuration file:
```
PLACEHOLDER
```

## Running Logstash
After creating the separate configuration file, I then ran logstash, using the "-f" flag to use my configuration file when running.
```
sudo /usr/share/logstash/bin/logstash -f /etc/logstash/conf.d/logstash_demo.conf
```