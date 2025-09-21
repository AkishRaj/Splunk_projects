# Analyzing DNS Log Files Using Splunk SIEM

## Overview
DNS (Domain Name System) logs provide crucial insights into network activity and are key for identifying potential security threats. Splunk SIEM (Security Information and Event Management) offers powerful capabilities to ingest, analyze, and visualize DNS logs, helping security analysts detect anomalies or malicious activities in real time.

This repository demonstrates how to upload and analyze DNS log files in Splunk, extract relevant information, identify suspicious activity, and generate actionable insights.

---

## Prerequriment
Before starting the analysis, ensure the following:

- A running **Splunk instance** (Splunk Enterprise or Splunk Cloud) is installed and configured.
- DNS log files are available in a suitable format (e.g., `.txt`, `.log`, `.csv`).
- Access to the Splunk web interface for uploading and querying data.

## Process Involved
 **Sample DNS Log File**: Contains DNS queries, responses, source IP, destination IP, etc.  
- **Splunk Add Data → Upload**: Upload log files to Splunk.  
- **Index & Sourcetype Configuration**: Assign index and sourcetype to uploaded logs.  
- **Event Ingestion**: Splunk stores and indexes log events.  
- **Search & Reporting / Analysis**: Retrieve and analyze events using Splunk SPL queries.  
- **Anomaly Detection / Threat Investigation**: Detect spikes, malicious domains, or abnormal patterns.  
- **Dashboards & Alerts**: Visualize DNS activity and configure alerts for suspicious events.

---

## Uploading Sample DNS Log Files to Splunk

Follow these steps to upload DNS log files into Splunk:

### 1. Prepare Sample DNS Logs
- Obtain DNS log files containing relevant events such as:
  - Source IP
  - Destination IP
  - Domain name (FQDN)
  - Query type (A, AAAA, MX, etc.)
  - Response code
- Save these files in a directory accessible by the Splunk instance.

### 2. Upload Log Files
1. Log in to the Splunk web interface.
2. Navigate to **Settings → Add Data → Upload**.
3. Click **Select File** and choose your DNS log file.

### 3. Configure Source Type and Index
- In **Set Source Type**, select an existing DNS sourcetype (e.g., `dns`) or create a custom one.
- Choose an appropriate **index** (default is usually `main`) or create a new index for DNS logs.
- Review host, source, and sourcetype settings to ensure accuracy.

### 4. Complete Upload
- Click **Review**, confirm the settings, and then click **Submit** to upload your file.

### 5. Verify Upload
To check that your DNS events are visible in Splunk, run:

index=<your_index> sourcetype=<your_sourcetype>
Replace <your_index> and <your_sourcetype> with the values assigned during upload.

## Analyzing DNS Logs in Splunk
Once your data is ingested, you can start analyzing DNS events.

1. Basic Search for DNS Events
Retrieve DNS events using:

index=* sourcetype=dns_sample
2. Extract Key Fields
Identify and extract relevant DNS fields such as:

src_ip → source IP

dest_ip → destination IP

query or fqdn → domain name

query_type → type of DNS query

dns_reply_code → response code

Example extraction using regex:

index=* sourcetype=dns_sample | regex _raw="(?i)\b(dns|domain|query|response|port 53)\b"

3. Detect Anomalies
Look for unusual patterns or spikes in DNS activity:

index=* sourcetype=dns_sample | stats count by fqdn

5. Identify Top DNS Sources
Count the most frequent queries and sources:

index=* sourcetype=dns_sample | top fqdn, src_ip

6. Investigate Suspicious Domains
Search for domains associated with malicious activity using threat intelligence:

index=* sourcetype=dns_sample fqdn="malicious.com"

## Best Practices
Regularly monitor DNS logs to identify abnormal spikes or unusual domain queries.
Integrate threat intelligence feeds to flag known malicious domains automatically.
Use Splunk dashboards to visualize DNS traffic trends over time.

## Conclusion
By leveraging Splunk SIEM for DNS log analysis, security professionals can efficiently monitor network activity, detect anomalies, and respond to potential threats. This workflow enhances organizational security posture and enables proactive threat detection.
