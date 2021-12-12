# Log4J Vulnerability

| Reference      | Severity | Date     |
|----------------|----------|----------|
| CVE-2021-44228 | 10/10    | 12/12/21 |

## Overview 
On Friday 10th December, Apache announced the discovery of a critical vulnerability in the LOG4J logging library for Java, which is used by millions of Java applications and other products and services to log error messages. Log4J is often installed on both Linux and Windows systems either directly, or often as a requirement of another package or system. 

**At 10/10 severity this is comfortably one of the most serious IT vulnerabilities to have been discovered in recent memory.**

The severity of the vulnerability is based on the ease with which this can be exploited – for example, a malformed username will be logged and can contain code which triggers the exploit. An attacker who can control log messages or log message parameters, can execute arbitrary code when message lookup substitution is enabled.

## Our Response
UKFast are encouraging all clients to validate whether their own applications and environments use Log4J and to upgrade to the latest version where possible, applying the appropriate mitigations where upgrade isn’t an option. For our part, UKFast are currently working through all our systems to be absolutely sure we are protected.

Our Security experts are running scans to identify attempts to exploit the vulnerability, and our support teams are looking at not only updating those products and services managed by UKFast but are also looking into the wider scope of affected applications, with a view to better informing our clients the best mitigation methods with systems they manage.

## Affected Versions
### Versions: (Log4j 2 requires Java 8 or greater at runtime)

|Version	                |Is it Vulnerable?                                 |
|---------------------------|--------------------------------------------------|
|Log4J v1.x  	            |EOL since 2015. Contains multiple vulnerabilities.|
|Log4J 2.0-beta9 to 2.10.0	|Yes - Mitigation available                        |
|Log4J 2.10 + 	            |Yes - Mitigation available                        |
|Log4J v2.15.0	            |Issue is mitigated in the latest version          |

## Mitigations

### In releases >=2.10
This behaviour can be mitigated by setting either the system property `log4j2.formatMsgNoLookups` or the environment variable `LOG4J_FORMAT_MSG_NO_LOOKUPS` to true.

### For releases from 2.0-beta9 to 2.10.0
The mitigation is to remove the **JndiLookup class** from the classpath: `zip -q -d log4j-core-*.jar org/apache/logging/log4j/core/lookup/JndiLookup.class.`

## Affected Software Information

| Vendor    | Service    | Impact               | Mitigation                                                    | Links |
|-----------|------------|----------------------|---------------------------------------------------------------|--------|
| AWS       | EC2        | Not Affected         | Amazon Linux 1 & 2 package versions are not affected.         | [1](https://aws.amazon.com/security/security-bulletins/AWS-2021-005/) |
| AWS       | WAF/Shield | Rule set available to improve detection | Apply `AWSManagedRulesKnownBadInputsRuleSet` to ACL | [1](https://aws.amazon.com/security/security-bulletins/AWS-2021-005/) |
| AWS       | OpenSearch | Affected             | AWS are updating all service domains                          | [1](https://aws.amazon.com/security/security-bulletins/AWS-2021-005/) |
| AWS       | Lambda     | Not Affected         | Does not include log4j in runtimes or containers              | [1](https://aws.amazon.com/security/security-bulletins/AWS-2021-005/) |
| AWS       | CloudHSM   | Affected < 3.4.1     | CloudHSM JCE versions earlier than 3.4.1, you may be impacted and should upgrade to version 3.4.1 or higher  | [1](https://aws.amazon.com/security/security-bulletins/AWS-2021-005/) |
| Jenkins   | Jenkins    | Core not Impacted, plugins might be | Identify if plugins are affected using the groovy script in link  | [1](https://www.jenkins.io/blog/2021/12/10/log4j2-rce-CVE-2021-44228/) |
| Elastic   | Elastic    | Not Affected         | Elasticearch versions 5.0.0+ contain a vulnerable version of Log4j, as well as the Security Manager which mitigates the attack. | [1](https://discuss.elastic.co/t/apache-log4j2-remote-code-execution-rce-vulnerability-cve-2021-44228-esa-2021-31/291476) |
| Elastic   | Logstash   | Logstash versions 6.8.x and 7.x up to 7.15.2, when configured to run on JDKs below 8u191 and 11.0.1, allow for remote loading of Java classes. | The widespread flag `-Dlog4j2.formatMsgNoLookups=true` does NOT mitigate the vulnerability in Logstash, as Logstash uses Log4j in a way where the flag has no effect. It is therefore necessary to remove the JndiLookup class from the log4j2 core jar, with the following command: `zip -q -d <LOGSTASH_HOME>/logstash-core/lib/jars/log4j-core-2.* org/apache/logging/log4j/core/lookup/JndiLookup.class` | [1](https://discuss.elastic.co/t/apache-log4j2-remote-code-execution-rce-vulnerability-cve-2021-44228-esa-2021-31/291476) |
| Elastic   | KIbana     | Not Affected         |                                                               | [1](https://discuss.elastic.co/t/apache-log4j2-remote-code-execution-rce-vulnerability-cve-2021-44228-esa-2021-31/291476) |
| Tomcat    | Version 5  | Not Affected         | Does not include log4j                                        | [1](https://access.redhat.com/solutions/6577191) |
| Tomcat    | Version 3  | Not Affected         | Disabled                                                      | [1](https://access.redhat.com/solutions/6577191) |
| Apache    | Solr       | Affected 7.4.0 to 7.7.3, 8.0.0 to 8.11.0     | Upgrade to 8.11.1 or greater          | [1](https://solr.apache.org/security.html#apache-solr-affected-by-apache-log4j-cve-2021-44228) |
| Java      | Java       | Java lower than 6u212, 7u202, 8u192 & 11.0.2 | Advised to upgrade Java version above the impacted versions | [1](https://www.veracode.com/blog/security-news/urgent-analysis-and-remediation-guidance-log4j-zero-day-rce-cve-2021-44228) |
| Java      | Log4j      | Affected between 2.0-beta9 to 2.14.1         | Upgrade to 2.15 to use mitigation     | [1](https://logging.apache.org/log4j/2.x/security.html) |
| Cpanel    | cpanel     | Affected             | Update for cpanel-dovecot-solr released. Those without auto updates will need to action manually | [1](https://forums.cpanel.net/threads/log4j-cve-2021-44228-does-it-affect-cpanel.696249/#post-2890493) |
| Vmware    | vCentre    |                      | https://kb.vmware.com/s/article/87081?lang=en_US | We have completed network changes to reduce impact from within eCloud from customers |
| Cisco     | ASDM       |                      | Upgrade serivces with ASDM.                                   | [1](https://tools.cisco.com/security/center/content/CiscoSecurityAdvisory/cisco-sa-apache-log4j-qRuKNEbd) |
| Unifi     |            |                      | Upgraded to latest candidate fix 6.5.54 see notes for link    | [1](https://community.ui.com/releases/UniFi-Network-Application-6-5-54/d717f241-48bb-4979-8b10-99db36ddabe1) |
| Jboss     |            |                      |                                                               |         |
| Microsoft |            |                      |                                                               | [1](https://msrc-blog.microsoft.com/2021/12/11/microsofts-response-to-cve-2021-44228-apache-log4j2/) |