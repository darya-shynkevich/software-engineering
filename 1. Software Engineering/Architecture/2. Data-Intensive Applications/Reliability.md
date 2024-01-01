1. The application performs the function that the user expected.
2. It can tolerate the user making mistakes or using the software in unexpected ways.
3. Its performance is good enough for the required use case, under the expected load and data volume.
4. The system prevents any unauthorized access and abuse.

***Reliability means making systems work correctly, even when faults occur.*** Faults can be in hardware (typically random and uncorrelated), software (bugs are typically systematic and hard to deal with), and humans (who inevitably make mistakes from time to time). Fault-tolerance techniques can hide certain types of faults from the end user.

The things that can go wrong are called **faults**, and systems that anticipate faults and can cope with them are called **fault-tolerant** or **resilient**.

### Hardware Faults
Disks may be set up in a RAID configuration, servers may have dual power supplies and hot-swappable CPUs, and datacenters may have batteries and diesel generators for backup power
This approach cannot completely prevent hardware problems from causing failures, but it is well understood and can often keep a machine running uninterrupted for years.

*In some cloud platforms such as Amazon Web Services (AWS) it is fairly common for virtual machine instances to become unavailable without warning, as the platforms are designed to prioritize flexibility and elasticityi over single-machine reliability.* Reference: https://web.archive.org/web/20160429075023/http://blog.awe.sm/2012/12/18/aws-the-good-the-bad-and-the-ugly/ 

### Software Errors
Such faults are harder to anticipate, and because they are correlated across nodes, they tend to cause many more system failures than uncorrelated hardware faults.

### Human Errors
Even when they have the best intentions, humans are known to be unreliable: 0. one study of large internet services found that configuration errors by operators were the leading cause of outages, whereas hard‐ ware faults (servers or network) played a role in only 10–25% of outages.

### Approaches:
1. Design systems in a way that minimizes opportunities for error. For example, well-designed abstractions, APIs, and admin interfaces make it easy to do “the right thing” and discourage “the wrong thing.” However, if the interfaces are too restrictive people will work around them, negating their benefit, so this is a tricky balance to get right.
2. Decouple the places where people make the most mistakes from the places where they can cause failures. In particular, provide fully featured non-production sandbox environments where people can explore and experiment safely, using real data, without affecting real users.
3. Test thoroughly at all levels, from unit tests to whole-system integration tests and manual tests. Automated testing is widely used, well understood, and espe‐ cially valuable for covering corner cases that rarely arise in normal operation.
4. Allow quick and easy recovery from human errors, to minimize the impact in the case of a failure. For example, make it fast to roll back configuration changes, roll out new code gradually (so that any unexpected bugs affect only a small subset of users), and provide tools to recompute data (in case it turns out that the old com‐ putation was incorrect).
5. Set up detailed and clear monitoring, such as performance metrics and error rates. In other engineering disciplines this is referred to as ***telemetry***. *(Once a rocket has left the ground, telemetry is essential for tracking what is happening, and for understanding failures)* Monitoring can show us early warning signals and allow us to check whether any assumptions or constraints are being violated. When a problem occurs, metrics can be invaluable in diagnosing the issue.
6. Implement good management practices and training.

### HOW TO?

**Network Failure** - network connection interruptions, high network latency, and routing errors. These problems hinder proper access to external resources and disrupt communication between applications and other systems.

How to detect?
1. Connection status: observe the connection status indicator light on the server or network device
2. Ping test
3. Traffic monitoring: utilize network traffic monitoring tools like `Wireshark` or `ntop` to observe network traffic conditions. Look for abnormal data packets, data packet loss, traffic congestion, or other irregularities.
4. Network latency test: employ network latency testing tools such as `ping`, `traceroute`, or `MTR` to check network latency. High latency can indicate network connection problems
5. Log Analysis: review the log files of servers and network devices for any errors or exceptions related to network connections. Log files often provide valuable information about network failures

How to Troubleshoot?
1. Check physical connections
2. Restart network device: this often clears cache and resets configurations
3. Check network configurations
4. Verify DNS settings: you can check DNS resolution by pinging the domain name or directly accessing the IP address
5. Check Firewall settings
6. Test other devices

**Server Failure** - hardware failures, operating system crashes, and service crashes. Such issues render systems unavailable or reduce their performance.

How to detect?
1. Unresponsiveness: if the server fails to respond to network requests or becomes unreachable, it may indicate a server failure.
2. Error logs: check the error log files on the server, including system logs and application logs. Look for error records related to server failures in these logs. They often contain valuable information to determine the cause of the failure.
3. Monitoring tools: use server monitoring tools to keep an eye on server performance indicators, such as CPU utilization, memory usage, and disk space. Abnormal metrics, such as high CPU or memory usage, may indicate server failure.

How to Troubleshoot?
1. Check server status: 
	1. the power indicator light should be on, showing that the server has power
	2. listen for the fan sound. The server’s fans should be running smoothly, as overheating can lead to performance issues
	3. the hard disk activity light may flicker as the server reads and writes data. Some servers have separate lights for individual hard drives. Ensure these lights are active, suggesting data access.
	4. inspect physical connections, including power, network cables, and data cables, to ensure they are securely attached and not loose or damaged.
2. Remote connection: attempt to connect to the server remotely using tools like SSH. Verify whether the remote connection can be established. If the connection fails, it may suggest issues with server software or network configuration.
3. Restart the server: try restarting the server as it can resolve temporary issues
4. Check hardware: inspect server hardware components. Ensure they are working properly and not malfunctioning
5. Check services and processes: examine the services and processes running on the server. Confirm whether critical services are running as expected and check for abnormal processes or zombie processes that might be causing issues.
6. Check logs
7. Restore backup data: if data on the server is damaged or lost, restore it from backups. Regularly back up your data and test the recoverability of backups to ensure data integrity.
8. Update and fix software
9. Contact technical support: if you are unable to resolve the server failure on your own, consider reaching out to the server supplier or a professional technical support team for expert guidance and assistance.

**Database Failure** -  database server crashes, connection errors, and data corruption. These issues disrupt an application’s ability to read or write data, leading to malfunctions or data inconsistencies.

How to detect?
1. Connection issues
2. Database Error Log: pay close attention to error codes and messages, as they can offer valuable clues about the nature of the failure.
3. Monitoring tools: 
	1. implement database monitoring tools to continuously monitor key performance indicators (KPIs) such as CPU utilization, memory usage, disk I/O, and query performance.
	2. abnormal metrics, such as high CPU usage or slow query response times, may be indicators of a database failure or performance issue.

How to Troubleshoot?
1. Check DB service status and restart if necessary
2. Remote connection test: 
	1. attempt to connect to the database remotely from the application server or another client machine to verify if the connection can be established
	2. if the remote connection fails, investigate potential network or database configuration issues
3. DB configuration check
4. Check DB space:
	1. examine the disk space usage of the database, including data files and log files
	2. ensure that there is sufficient disk space available to prevent database failures caused by running out of space
5. Log Analysis: 
	1. analyze the database log files, including transaction logs and error logs
	2. look for abnormal records related to faults, which may include database errors, deadlocks, or log corruption
6. DB health check: run database health check tools provided by your database system (e.g., Oracle’s `DBVERIFY`, MySQL’s `CHECK TABLE`). These tools can help detect and repair physical corruption or consistency issues within the database.
7. DB repair and recovery: 
	1. if the database file is corrupted or has data consistency problems, use database repair tools or recovery operations
	2. this may involve repairing corrupted data files, restoring data from backups, or applying database transaction logs to recover lost data
8. DB parameter adjustment: fine-tune settings to optimize performance and stability
9. DB performance tuning: optimize query statements, adjust indexes, allocate more hardware resources, and fine-tune database parameters to enhance response times and throughput
10. DB version upgrade or patching
11. Contact technical support

**Software Errors** - application bugs, configuration errors, and dependency issues. They result in application crashes, malfunctions, or performance degradation.

How to detect?
1. Application error messages
2. Abnormal application behavior
3. User feedback

How to Troubleshoot?
1. Reproduce the problem
2. Log analysis
3. Debugging tools
4. Fix the code
5. Env and configuration: verify that the software’s environment and configuration settings are correctly configured. Check libraries, versions, file permissions, and other dependencies to ensure they align with the software’s requirements
6. Updates and fixes: 
	1. regularly check for software updates, patches, or fixes provided by the software manufacturer or open-source community
	2. manufacturers frequently release updates to address known issues, enhance stability, and improve security. Keeping your software up to date is essential for bug prevention.

**Security Vulnerabilities or Attacks** - malicious behavior, such as unauthorized access, data breaches, and denial-of-service attacks.

How to detect?
1. Security audits and scans: 
	1. regularly perform security audits and scans using professional security tools. These tools are designed to identify potential vulnerabilities in your systems and applications
	2. security audits and scans can pinpoint known security vulnerabilities and offer recommendations for remediation. It’s crucial to choose the right vulnerability scanning tools that align with your specific needs
2. Security log analysis: 
	1. analyze security logs, including those from the operating system, network devices, and applications. Look for signs of unusual activity, login attempts, denial of service attacks, or any suspicious behavior
	2. thoroughly reviewing logs is essential for identifying potential security threats and vulnerabilities
3. Weakness exploitation detection: implement `intrusion detection systems` (IDS) or `intrusion prevention systems` (IPS) to monitor network traffic and system activities. These systems provide real-time monitoring to detect and respond to signs of attacks or attempts to exploit weaknesses in your systems.
4. Vulnerability Disclosures and Security Bulletins: stay updated by regularly monitoring security bulletins and vulnerability disclosures from software and system vendors. These sources often highlight known security vulnerabilities and attack vectors, helping you identify and patch potential issues.

How to Troubleshoot?
1. Review system and application configuration
2. Review access controls and permissions
3. Network traffic monitoring and analysis (Akamai)
4. Malicious code scan (bandit)
5. Apply security patches and updates promptly
6. Data encryption: encrypt sensitive data, both during transmission and storage, using strong encryption algorithms and protocols. This ensures data remains confidential in case of a security breach
7. Strengthen network security defense measures: configure and manage firewalls, IDS, IPS, and security gateways to prevent unauthorized access and malicious traffic
8. Security auditing and monitoring
9. Strengthen employee security awareness training
10. Regular vulnerability assessments and penetration testing: conduct regular vulnerability assessments and penetration testing to identify weaknesses in your systems and applications. Early detection and resolution of security issues are vital
11. Enhance security compliance: ensure that your systems and applications comply with relevant security standards and regulatory requirements. Conduct regular compliance assessments to identify and correct any non-compliance issues
12. Establish a disaster recovery and business continuity plan: develop a plan to deal with the impact of security incidents and attacks. This includes backing up important data and testing the effectiveness of the recovery process
13. Seek professional security support
14. Implement network isolation and security segmentation: divide your network into different security areas and enforce network isolation policies. This reduces an attacker’s ability to spread and move within the system
15. Enhance log management and analysis: configure systems and applications to generate detailed log records and establish log management and analysis mechanisms. Real-time log monitoring helps detect abnormal activities and potential security threats
16. Strengthen physical security measures
17. Strengthen supply chain security: review and evaluate the security measures of suppliers and third-party partners to ensure they meet security standards.
18. Timely response and recovery: establish response and recovery plans to deal with security incidents or attacks. Act swiftly to isolate affected systems, collect evidence, and remediate vulnerabilities.

**Storage Failure** - disk failures, storage device issues, and data loss. These problems lead to unusable data, corrupted files, or data irrecoverability.

How to detect?
1. Check storage device lights
2. Monitor storage devices: utilize monitoring tools or third-party monitoring solutions provided by the storage device to keep an eye on its health status and performance indicators in real-time. These metrics may include disk usage, I/O latency, transfer rates, and more
3. Watch the System Error Log: inspect the server or storage device error log for storage-related error messages or alerts. These logs typically record information about storage device failures, disk errors, transmission errors, and other relevant data
4. Monitor application errors

How to Troubleshoot?
1. Verify storage connection
2. Check disk status: to confirm whether there is a disk failure or damage, check the disk status within the storage device. Some storage devices offer management interfaces or command-line tools to view disk health status and SMART (Self-Monitoring, Analysis, and Reporting Technology) information
3. Run storage diagnostic tools
4. Restart storage devices and servers: follow the device manufacturer’s instructions for proper rebooting procedures to avoid potential data loss or system instability
5. Data recovery and backup:
	1. if data on a storage device is affected or inaccessible due to a failure, consider data recovery operations. Additionally, maintaining regular backups of your data can significantly reduce the risk of data loss
	2. data recovery experts can assist in recovering lost data, while well-planned backup strategies ensure data availability
6. Replacing a failed disc
7. Repair file system errors: in cases where there are errors in the file system on your storage device, attempt to repair it. Use an appropriate file system repair tool or the disk check and repair commands provided by the operating system
8. Expand storage capacity
9. Data migration and reconstruction
10. Seek manufacturer support

**Configuration Errors** - cause systems to operate incorrectly due to settings such as port settings, permission settings, or network configurations.

How to detect?
1. Monitor system logs and error reports
2. User feedback and reports
3. Testing and validation

How to Troubleshoot?
1. Review configuration files
2. Check env variables and command line params
3. Backup Configuration !

**Third-Party Service Failure** - when these services fail, the application may not function correctly or may have limited features.

How to detect?
1. Monitor service status: subscribe to alert notifications offered by your service provider to receive immediate alerts in case of a service outage or performance degradation
2. User feedback and reports
3. Monitor logs and error reports

How to Troubleshoot?
1. Confirm the scope of the problem:
	1. determine whether the service failure is limited to your application or if it broadly affects other users. Checking if other users have reported similar issues can help you identify the scope of the problem
	2. if multiple users are experiencing the same problem, it’s likely an overall malfunction with the third-party service
2. Check network connectivity and integration configuration
3. Check the third-party provider's status page
4. Contact the support of the third-party provider
5. Restart the service: try restarting your application or service to check if that resolves the issue. In some cases, failures may be caused by temporary connectivity issues or unstable service status, and a simple restart can resolve them
6. Find alternative solutions: if the third-party service cannot be quickly restored or the issue remains unresolved, consider searching for alternative services or solutions
7. Implement a backup plan

### References:

1. [What Bugs Live in the Cloud?](https://ucare.cs.uchicago.edu/pdf/socc14-cbs.pdf)
2. [How Complex Systems Fail](https://www.adaptivecapacitylabs.com/HowComplexSystemsFail.pdf)
3. [Getting Real About Distributed System Reliability](https://blog.empathybox.com/post/19574936361/getting-real-about-distributed-system-reliability)
4. [Why do Internet services fail, and what can be done about it?](https://static.usenix.org/events/usits03/tech/full_papers/oppenheimer/oppenheimer.pdf)
5. [Simple Testing Can Prevent Most Critical Failures: An Analysis of Production Failures in Distributed Data-Intensive Systems](https://www.usenix.org/system/files/conference/osdi14/osdi14-paper-yuan.pdf)
6. [The Human Impact of Bugs](http://jury.me/blog/2013/3/14/the-human-impact-of-bugs)
7. [Understanding and Addressing Common Production Failures in Systems and Applications](https://jinlow.medium.com/understanding-and-addressing-common-production-failures-in-systems-and-applications-3f045ed2c24a)
8. 