## Table of Contents
- [Introduction to SOC and Phishing Threats](#introduction-to-soc-and-phishing-threats)
- [The Role of SOC in Phishing Detection](#the-role-of-soc-in-phishing-detection)
- [Alert Investigation: Identifying Phishing Emails](#alert-investigation-identifying-phishing-emails)
- [Threat Intelligence & Indicators of Compromise](#threat-intelligence--indicators-of-compromise)
- [Handling False Positives & True Positives](#handling-false-positives--true-positives)
- [In-depth Analysis of Malicious Attachments](#in-depth-analysis-of-malicious-attachments)
- [Confirming a Phishing Attack](#confirming-a-phishing-attack)
- [Incident Documentation & Escalation](#incident-documentation--escalation)
- [SOC Performance Metrics & Lessons Learned](#soc-performance-metrics--lessons-learned)
- [Incident Reporting and Response](#incident-reporting-and-response)
- [Improving SOC Efficiency](#improving-soc-efficiency)
- [Conclusion](#conclusion)



## Introduction to SOC and Phishing Threats

A Security Operations Center (SOC) is responsible for monitoring, detecting, and responding to cybersecurity threats in real-time.

One of the most common cyber threats handled by SOC analysts is phishing attacks, where attackers attempt to deceive users into revealing

sensitive information or downloading malicious files. In this article, we will explore how SOC analysts identify and investigate phishing emails

using tools like ANYRUN, Splunk, VirusTotal, and sandbox analysis.

## The Role of SOC in Phishing Detection

SOC analysts use phishing detection rules and security alerts to identify suspicious emails that may contain malicious links

attachments, or social engineering tactics. These emails are flagged based on:

Unusual sender domains

Deceptive subject lines

Embedded links leading to phishing sites

Attachments that contain malware

Once flagged, analysts investigate the email metadata, check sender authenticity, and examine attached files to determine whether

the alert is a false positive or a true positive.

## Alert Investigation: Identifying Phishing Emails

The SOC analyst starts by logging into the dashboard, where two phishing alerts are visible. Each alert contains email metadata

including sender, recipient, and the email subject line, which often features social engineering tactics such as fake prize winnings or urgent financial requests.

## Threat Intelligence & Indicators of Compromise

To investigate the phishing emails:

Email headers & sender domains are analyzed to check if they originate from known bad sources.

Splunk is used to correlate the email domains and user interactions.

                          
  ![soc2 Screenshot](./Screenshot%20(140).png)


 in the search bar of splunk you can search for a domain name a file a parent directory and much more.In addition, the pic 
 
 below shows that you can also analyze logs by time stamps and date

  ![soc2 Screenshot](./Screenshot%20(140).png)
                                
Threat intelligence tools such as VirusTotal and Anom Sandbox help identify malicious domains or attachments.


## Handling False Positives & True Positives

Most alerts turn out to be false positives due to poorly tuned detection rules. The SOC analyst documents findings

marking them as safe while ensuring detection rules are refined to prevent unnecessary alerts in the future.

However, some cases reveal true positives, requiring further analysis:

The SOC team extracts email attachments and analyzes script contents to check for obfuscated code.

Parent-child process relationships are examined to detect malware execution.

   ![soc3 Screenshot](./Screenshot%20(138).png)

## In-depth Analysis of Malicious Attachments

A suspicious PDF file attached to one phishing email is flagged. The investigation follows these steps:

Extract file hash using PowerShell to check against known malware signatures.

Analyze file contents using Splunk and threat intelligence platforms.

Monitor system logs to determine if the PDF triggered malicious actions post-opening.

## Confirming a Phishing Attack

The investigation reveals that opening the PDF triggered a PowerShell command, downloading a privilege escalation tool (PowerCat)

which connected to a Command-and-Control (C2) server. This confirms the alert as a true positive.

## Incident Documentation & Escalation

A detailed case report is written, explaining the phishing technique, how the malware bypassed detection, and the steps taken to mitigate it.

The case is escalated for further action, ensuring future preventative measures are implemented.

## SOC Performance Metrics & Lessons Learned

The SOC analyst’s response time and detection accuracy are reviewed.

The biggest delay was handling phishing email alerts, emphasizing the need for faster triage methods.

Metrics like true positive rate (TPR) and dwell time help gauge SOC efficiency.

## Incident Reporting and Response

Once a phishing attack is confirmed, the SOC team documents the investigation findings and escalates the case for further remediation. The report includes:

Nature of the attack (e.g., social engineering, malware delivery, credential harvesting).

Indicators of compromise (IoCs) such as malicious IPs, domains, and file hashes.

Recommended security actions, like blocking the sender’s domain and updating detection rules.

## Improving SOC Efficiency

To enhance SOC efficiency, analysts should:

Refine detection rules to reduce false positives.

Use automated threat intelligence tools for faster phishing detection.

Correlate logs in Splunk to uncover hidden attack patterns.

Train employees to recognize phishing attempts, reducing successful attacks.

# #Conclusion

Refining detection rules reduces false positives.

Threat intelligence tools are essential for domain and attachment analysis.

Time-based investigation using Splunk helps track malware execution.


SOC analysts must document findings clearly for effective escalation.
