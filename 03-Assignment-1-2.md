# Prisma Cloud Ultimate Test Drive (UTD) Workshop
## Assignment 1.2
### Prisma Cloud Code to Cloud Attack Path Alert & Remediation
To understand the true security risk of an application or infrastructure requires a complete assessment and correlation of a broad set of security signals. For example, if a virtual machine or an application is vulnerable to CVE-1234 that is network exploitable, is internet exposed, and has overly permissive IAM access to sensitive data, then this combination presents a relatively critical or high risk compared to an instance that contains the same vulnerability but is not internet exposed.

The Attack Path policies are out-of-the-box policies that are enabled by default. These policies identify the confluence of issues that increase the likelihood of a security breach and are based on relationships such as, overly permissive identities, permissions, network exposure, infrastructure misconfiguration, and vulnerabilities that would enable an attacker to target your application. Prisma Cloud helps you identify attack paths and presents them in a graph view, offering valuable security context to protect against high-risk threats, which often requires you to take immediate action.

In this activity, you will:
* View Alert on risky SQL, Code injection and other attacks.
* Analyze the attack paths and the resources involved in them.
* View how Prisma Cloud can be leveraged to remediate or block the attacks.

Note: This is a standalone activity and is not dependent on other activities.
Hint: As you navigate the alerts view in the Prisma Cloud console you can click the Add filters button to enable/disable the needed filters

#### Prisma Cloud Code to Cloud Attack Path Alert
1. Navigate to **Prisma Cloud Enterprise Edition > Cloud Security > Alerts > Overview.**
2. Set the following filters (you can add additional filter by clicking on the Add Filter button):
    * Time Range = All Time
    * Policy Name = SQL and Code Injection attacks were detected
    
    ![alt text](/resources/pcs-screen-23.png)

Notes: SQL and Code Injection attacks were detected is a custom search policy and Alert rule that was created for the purpose of the lab by leveraging out of the box Prisma Cloud policies. This is done for ease of use and convenience of lab experience. As a user, you can also create custom policy as such to meet your organization needs.
3. Click on the **SQL and Code Injection attacks were detected** result and click on the Sales and Trading cnsp-app4 under Asset Name column.
    
 ![alt text](/resources/pcs-screen-24.png)

4. This should bring up details about this instance and contains more findings and information about the attacks.The Overview tab contains the overview of this VM. Clicking on the Attack Path will reveal how the attack occurred and the resources involved in it.
Note: If you do not see the same graph as in the below screenshot, then it’s possible that the resource is involved in more than once incident and an alert. You can change the view by clicking on the Alert dropdown menu and selecting SQL and Code Injection attacks were detected as shown in the below screenshot

    ![alt text](/resources/pcs-screen-25.png)
    ![alt text](/resources/pcs-screen-26.png)

5. Within the graph, you can see the traffic flow and the complete attack path. The traffic enters through the **Internet Gateway** and hits the **Sales and Trading cnsp-app4** EC2 instance. This instance has an IAM role **sales-trading-admin-role-cnsp-app4** attached to it, which has wildcard access to S3 bucket and also access to **prisma-cloud-pcds-bucket** S3 Bucket. As this instance is vulnerable to SQL and Code injection attacks, the attacker can potentially perform a Data exfiltration from the S3 bucket through the compromised host. This is represented by the Attack Path.
6. Within the graph, clicking on the Vulnerabilities Icon and clicking on any Vulnerability will provide more information about the vulnerability. Click on a Vulnerability and thereafter click on View Details will open an additional findings sidecar.

    ![alt text](/resources/pcs-screen-27.png)
    ![alt text](/resources/pcs-screen-28.png)

7. Clicking on Assets will show more information about the affected assets. Once done, close the CVE Sidecar.
    
    ![alt text](/resources/pcs-screen-29.png)

8. Clicking on Audit Trail will show more information about what changes occurred at the resource level and the timeline. Alerts tab will show all the alerts that are that this resource is currently involved in. The Findings tab shows the findings for this specific resource.
    
    ![alt text](/resources/pcs-screen-30.png)

9. Within the Sales and Trading cnsp-app4 VM sidecar, click on the Vulnerabilities tab to see all the Vulnerabilities that were detected for this VM. Further clicking on options such as Critical & High , Exploitable and Patchable will filter the results.
    
    ![alt text](/resources/pcs-screen-31.png)

10. Navigate back to the Attack Path graph (and make sure that the “SQL and Code Injection attacks were detected” alert is selected). Let’s further investigate the Attack Path in the graph and examine the blast radius.
    
    ![alt text](/resources/pcs-screen-32.png)

11. Within the graph, click on the S3 Bucket prisma-cloud-pcds-bucket by clicking on it and clicking on View Details

    ![alt text](/resources/pcs-screen-33.png)
    ![alt text](/resources/pcs-screen-34.png)

    * Here you can see the S3 Bucket details. Head over to the Alerts tab in the S3 Bucket details and here you will see that there’s an alert for this bucket Storage Asset with sensitive data found. Do not click on the Policy Name as clicking on the Policy Name as you will be navigated to another page. Once done looking at Alerts, close the Alert window

    ![alt text](/resources/pcs-screen-35.png)

    * Within the S3 Bucket page, click on Objects. Here you can see the list of all the objects that reside in that S3 Bucket and these are at the risk of being exfiltrated as a result of the SQL and Code injection attacks. If you click on PII_Health_IP_with_multiline.txt.txt and head over to Attributes, you can see that this file contains sensitive Personal Identifiable information.

    ![alt text](/resources/pcs-screen-36.png)
    ![alt text](/resources/pcs-screen-37.png)

    * Here, Prisma Cloud has identified that there’s sensitive data stored within the S3 Bucket that the compromised host has access to and is at risk of data exfiltration. To identify and detect confidential and sensitive data, Prisma Cloud Data Security integrates with Palo Alto Network's Enterprise DLP service and provides built-in data profiles, which include data patterns that match sensitive information such as PII, health care, financial information and Intellectual Property.
    * On the bucket, click on any file, it will show you more findings

    ![alt text](/resources/pcs-screen-38.png)

12. Close the Snippets pop up. Close the Object sidecar and close the S3 bucket sidecar. Close the Sales and Trading cnsp-app4 sidecar
    
    ![alt text](/resources/pcs-screen-39.png)

13. In the SQL and Code injection attacks were detected Alert window, click on the value corresponding to the Alert ID column. In the next windows, click on the Evidence tab.

    ![alt text](/resources/pcs-screen-40.png)
    ![alt text](/resources/pcs-screen-41.png)

14. Click on the SQL Injection and click on View Additional Finding Details. You will now be directed to the Prisma Cloud Runtime Security WaaS (Web Application and API Security) console, which was responsible for detecting this attack. Here we will be able to see more information about the attack.
Note: to add Date to the filter with start date:1 Nov 2023 and end date: current.

    ![alt text](/resources/pcs-screen-42.png)

Note: Below screenshot might look different in your case as some menus are collapsed to fit the screenshot in the page.

![alt text](/resources/pcs-screen-43.png)

15. Click on the SQL Injection number to bring up the Aggregated WaaS Events for that attack type.

    ![alt text](/resources/pcs-screen-44.png)

16. As you can see, attacks are detected and the effect is set to Alert. This can be changed to Prevent to prevent the attacks. But we will not be performing that action in this lab but we will show how it can be achieved.
17. If you scroll further down, you can see which rule is responsible for detecting the attacks and this would be finance-eks-cluster-waas-policy. Click on the rule and select Yes for the pop up dialogue.

    ![alt text](/resources/pcs-screen-45.png)

18. Click on the rule and click on the item under App List

    ![alt text](/resources/pcs-screen-46.png)

19. Click on the App Firewall tab and you should see the SQL Injection and other items (Grayed out because the lab role is a read-only role and doesn’t have permissions to change settings), which can be set to Prevent or Ban to prevent these attacks

    ![alt text](/resources/pcs-screen-47.png)

All the findings contributed to an attack path, which might lead to a data breach in the customer environment. Prisma Cloud detects what might potentially go wrong (misconfiguration) and what has already gone wrong (actual attack), and combining these information on the same single plane of glass. By having this information detected by Prisma Cloud, users can choose different ways to remediate the risks discovered.

20. Click on the App Firewall tab and you should see the SQL Injection and other items (Grayed out because the lab role is a read-only role and doesn’t have permissions to change settings), which can be set to Prevent or Ban to prevent these attacks

    ![alt text](/resources/pcs-screen-47.png)

#### Prisma Cloud Code to Cloud Remediation
Identifying critical risks is just the beginning. Prisma Cloud assists with eliminating the root cause through its unique Code to Cloud remediation capability. This approach enables security teams to address the issue at its source.

1. Click on triangle icon with **AWS EC2 Instance not configured with Instance Metadata Service v2 (IMDSv2)**
    
    ![alt text](/resources/pcs-screen-48.png)

2. On the popped out window, it shows a few different way to remediate the misconfiguration:
    * Fix in Cloud - click the **Fix in Cloud** button, allowing the user to fix the misconfiguration directly on the cloud environment, by either click on **Execute Command** button, or copying the CLI command and run it on cloud shell. 
    
    ![alt text](/resources/pcs-screen-22.png)
    
    Note: You do not need to click on the execute command button
    * Fix in Code - click on the **Fix in Code** button, allowing the user to fix the misconfiguration on Infrastructure as Code (IaC).
    * Manual Fix - click on the **Manual Fix** button, allowing the user to open a ticket for this misconfiguration so that the right team (workload owner) could assist on remediating the misconfiguration.
By having different actionable items on a single plane of glass allows security team to review and react to any attack path detected, and reduce or close off the attack path, so that the security risks can be minimized. 

#### Prisma Cloud CIEM
Cloud Infrastructure Entitlement Management (CIEM) provides users with broad visibility into effective permissions, continuously monitors multicloud environments for risky and unused entitlements, and automatically makes least privilege recommendations.

1. Click on identity/role icon with **sales-trading-admin-role-cnsp-app** and select View Details

    ![alt text](/resources/pcs-iam-screen-1.png) 

2. Click on Alerts tab and the Alert of policy **AWS IAM Groups and Roles with IAM Metadata Write permsssions are unused for 90 days**

    ![alt text](/resources/pcs-iam-screen-2.png)    

3. Review Remediation Guide and click on **Investigate** button

    ![alt text](/resources/pcs-iam-screen-3.png) 

4. Review the Actions and Last Access to observe the various write permissions attached and the last usage time

    ![alt text](/resources/pcs-iam-screen-4.png)    

There are also other attack path alerts that you can view via the alert page, feel free to run through them. Once you're done, you can move on to the next section [here](/04-Assignment-2-1.md)
