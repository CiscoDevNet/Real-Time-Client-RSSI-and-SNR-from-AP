# Real-time RSSI & SNR Graph from AP perspective
First presented at Cisco Live! Las Vegas 2016 [BRKEWN-3000]
(https://www.ciscolive.com/online/connect/sessionDetail.ww?SESSION_ID=90808&backBtn=true)

# Brief
This project uses debug command output from the AP command line to generate a near real-time graph of the client's RSSI & SNR from the AP's perspective. 

# Caveats
 * The ap_debug_Rcv.expect script should never be run against a production access point; The debug commands enabled by this script **WILL** degrade performance.

# Prerequisites
 * ELK stack:
    * [ElasticSearch] (https://www.elastic.co/products/elasticsearch)
    * [Logshash] (https://www.elastic.co/products/logstash)
    * [Kibana] (https://www.elastic.co/products/kibana)
 * Cisco AccessPoint
 * Expect

# Installation
 1. Extract source code files into a folder
 2. Edit ap_debug_Rcv.expect and replace ##AP_Username## and ##AP_Password## with your AP username and password, respectively
 3. Ensure SSH access is enabled on the access point from the WLC GUI


# Usage
1. Start the live feed to elasticsearch with the command 
```Shell
./ap_debug_Rcv.expect <AP IP Address> | logstash -f ap_debug_RCV_logstash-pipeline.conf
```
2. From the Kibana GUI add a new line chart Visualization for a new search
3. Create a Y-axis with Aggregation:Average Field:rssi Label:RSSI
4. Create a Y-axis with Aggregation:Average Field:snr Label:SNR
5. Set the X-axis to Aggregation:Date Histogram Field:@timestamp Interval:Second
6. In the top right of the screen, set the time period to Relative: From: 5 minutes ago To: now
7. Also in the top right, set the refresh period to every 5 seconds
8. Enter the Last-6 of the Client MAC Address into the search field and start the search
9. Observe!

# Compatibility
This script was tested with: 
 * OS X El Capitan Ver. 10.11.5
 * ### AP model
 * ### WLC Version
 * Expect v5.45
 * Elasticsearch v2.3.3
 * Logstash v2.3.3
 * Kibana v4.5.1
 
