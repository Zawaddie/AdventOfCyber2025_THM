# DAY 03: Splunk Basics - Did you SIEM?

Splunk for log analysis

## Exploring the Logs

We start a re-configured VM by clicking the Start Machine button and then connect to the Splunk SIEM by visiting **https://LAB_WEB_URL.p.thmlabs.com** in the browser.

In the Splunk instance, the data has been pre-ingested for us to investigate the incident. 

---
![splunk00](images/splunk00.png)

On the Splunk interface, we click on **Search & Reporting** on the left panel, and then type **index=main** in the search bar to show all ingested logs.
We also select All time as the time frame from the dropdown on the right of the search bar.

After running the query, we will are presented with two separate datasets that have been pre-ingested into Splunk. We can verify this by clicking on the sourcetype field in the fields list on the left of the page
The two datasets are as follows:

- **web_traffic:** contains events related to web connections to and from the web server.
- **firewall_logs:** contains the firewall logs, showing the traffic allowed or blocked. The local IP assigned to the web server is **10.10.1.15.**
  We then explore these logs and investigate the attack on our servers to identify the culprit.

![splunk01a](images/splunk01a.png)
![splunk02a](images/splunk02a.png)

---



![splunk00](images/splunk00.png)

![splunk00](images/splunk00.png)

![slunk00](images/splunk00)

![slunk00](images/splunk00)

![slunk00](images/splunk00)

![slunk00](images/splunk00)
![slunk00](images/splunk00)![slunk00](images/splunk00)
![slunk00](images/splunk00)
![slunk00](images/splunk00)
![slunk00](images/splunk00)
