![unsw_0](https://user-images.githubusercontent.com/7439960/197022279-cfdf58ea-01ca-4c1e-854e-99776012a0bd.png) 
# Time2Fix Prediction Dataset

**Authors**  
  Carlos A. Rivera A.(1), Xinzhang Chen(1), Arash Shaghaghi1(1), Gustavo Batista(1), and Salil S. Kanhere(1)  
   1. The University of New South Wales (UNSW) Sydney, Australia.  
  
  
**Abstract**  
The rapid integration of Internet of Things (IoT) devices into enterprise environments presents significant security challenges. Many IoT devices are released to the market with minimal security measures, often harbouring an average of 25 vulnerabilities per device. To enhance cybersecurity measures and aid system administrators in managing IoT patches more effectively, we propose an innovative framework that predicts the time it will take for a vulnerable IoT device to receive a fix or patch. We developed a survival analysis model based on the Accelerated Failure Time (AFT) approach, implemented using the XGBoost ensemble regression model, to predict when vulnerable IoT devices will receive fixes or patches. By constructing a comprehensive IoT vulnerabilities database that combines public and private sources, we provide insights into affected devices, vulnerability detection dates, published CVEs, patch release dates, and associated Twitter activity trends. We conducted thorough experiments evaluating different combinations of features, including fundamental device and vulnerability data, National Vulnerability Database (NVD) information such as CVE, CWE, and CVSS scores, transformed textual descriptions into sentence vectors, and the frequency of Twitter trends related to CVEs. Our experiments demonstrate that the proposed model accurately predicts the Time-2-Fix for IoT vulnerabilities, with data from VulDB and NVD proving particularly effective. Incorporating Twitter trend data offered minimal additional benefit. This framework provides a practical tool for organisations to anticipate vulnerability resolutions, improve IoT patch management, and strengthen their cybersecurity posture against potential threats.

**Keywords**  
CyberSecurity in IoT | Time2Fix | Internet of Things (IoT) | Common Vulnerability Enumeration (CVE) | Common Weakness Enumeration (CWE) | MITRE Att&ck |


**The Datasets**  
Our dataset was created by collecting vulnerability information on IoT devices from four sources: [MITRE](https://cve.mitre.org/), [NIST NVD](https://nvd.nist.gov/), [VulDB](https://vuldb.com/), and [Twitter](https://twitter.com). MITRE provided information about common vulnerabilities and exposures (CVE) and their corresponding release dates. NIST delivers additional data to each CVE, such as the common vulnerability risk score (CVSS) and the common weaknesses enumeration (CWE). VulDB was used to obtain extensive information on the vulnerability (CVE), including risk scores (CVSS), exploits, attacks, weaknesses (CWEs), remediation actions, and dates corresponding to each element. Lastly, Twitter was used to identify social network trends related to each published vulnerability identifier (CVEID) and extracted only the trend counts provided by the platform. Table 1 provides a detailed breakdown of the features we extracted from each source.

We used the MITRE and NIST vulnerabilities databases (NIST NVD) to gather vulnerabilities. We filtered out records not related to hardware by identifying the type of affected product using the product code in the NVD (_o_ for operating system, _s_ for software, _a_ for application, and _h_ for hardware). We utilised the publication date of CVE as a reference to determine the Time-2-Fix for each vulnerability. However, we came across numerous records that resulted in a zero Time-2-Fix. This indicates that the date of the fix being published is the same as that of CVE. There could be various reasons behind this, and one of the most common reasons we found is the manufacturer's strategy, which involves publishing CVEs only after developing the fix. Due to this biased process, we conducted an in-depth analysis of the CVE data. We discovered that NVD labels the references attached to each CVE, and we found it convenient that one of the labels refers to PATCH (Vendor Advisory, Patch, Third Party Advisory). After developing Java code scripts, the dataset underwent filtration procedures, with the intention of retaining only those records that were accurately labelled as PATCH. 

To accurately gauge the length of Time-2-Fix, we thoroughly investigated the reference links associated with each CVE. We meticulously searched for relevant information regarding the date the fix was made available. Once we obtained this information, we compared it to the date the vulnerability had been initially detected. By subtracting the latter from the former, we could precisely calculate the time (Time-2-Fix) it had taken for the device's manufacturer to address the reported vulnerability (CVE). The calculated Time-2-Fix is what we will use as the predicted feature. 

Upon compiling all pertinent information, our final dataset consisted of 1,027 records. These records encompass many aspects, including vulnerabilities, exploits, remediation actions, risk levels, timelines, and Twitter trends regarding IoT devices. We thoroughly analysed this dataset to identify the optimal model for implementation. In the next section, we delve deeper into the chosen machine learning model, which accurately predicts Time-2-Fix.


**Table 1. Dataset-0 features.**
Breakdown of the features provided by each source: MITRE (SRC-1), NIST (RC-2), VulDB (SRC-3), and Twitter (SRC-4).
|Source |Feature Name |Data Type |Unique Values|Details|
| --- | --- | --- | --- | --- |
| SRC-1 | CVEID | Continuous              |  1,022 (21-02/2023)     | Vulnerability identifier.| 
| SRC-2 | CVSS |  Categorical             |  100           | A real number range 0 to 10.| 
| SRC-2 | Weakness ID (CWEID) |Categorical|  926     | Weakness identifier| 
| SRC-3 | Title |  Categorical            |  885     | Vulnerability Title.| 
| SRC-3 | Summary |  Categorical          |  271     | Vulnerability Summary.| 
| SRC-3 | Affected |  Categorical         |  871     | Affected products description.| 
| SRC-3 | Vulnerability |  Categorical    |  73     | Vulnerability details identifier.
| SRC-3 | Impact |  Categorical           |  19     | Attack impact.| 
| SRC-3 | Exploit |  Categorical          |  91     | Exploit description.| 
| SRC-3 | Countermeasure |  Categorical   |  194     | Suggested countermeasure description.| 
| SRC-3 | Sources |  Categorical          |  735     | Data sources description.| 
| SRC-3 | Vendor |  Categorical           |  77     | Vendor name of affected product.| 
| SRC-3 | Product Name |  Categorical     |  333     | Name of affected product.| 
| SRC-3 | Component |  Categorical        |  360     | Name of affected component.| 
| SRC-3 | Version |  Categorical          |  392     | Affected product version.| 
| SRC-3 | Risk |  Categorical             |  3     | Vulnerability Risk (Low, Medium, High).| 
| SRC-3 | Class |  Categorical            |  72     | Vulnerability Class Name.| 
| SRC-3 | Attck |  Categorical            |  21     | Attck Identifier from MITRE Att&ck.| 
| SRC-3 | CVE Assigned | Date             |  235  | Date of the CVE assignment.| 
| SRC-3 | VulDB Base Score | Categorical  |  100     | CVSSv3 Base Score calculation by VulDB.| 
| SRC-3| Vulnerability Found| Date        |  144     | Timestamp for reported vulnerability found.| 
| SRC-3 | Advisory Disclosed| Date        |  326     | Timestamp for reported vulnerability Disclosed by Advisory.| 
| SRC-3 | CVE Reserved |  Date            |  235     | Timestamp CVE Reserved.| 
| SRC-3 | NVD Disclosed |  Date           |  215     | Timestamp of Vulnerability Disclosure by NVD.| 
| SRC-3| VulDB Entry Created| Date        |  171     | Timestamp of Vulnerability Entry Creation by VulDB.| 
| SRC-3| VulDB Entry Updated | Date       |  1022     | Timestamp of Vulnerability Entry Last Updated by VulDB.| 
| SRC-3| Advisory Confirmation| Date      |  3     | Advisory Confirmation Type (Confirmed, Not Defined, Uncorroborated.| 
| SRC-3 | Exploit Publicity |  Categorical|  3     | Publicity of the exploit (Public, Private, NA).| 
| SRC-3| Exploit Availability| Categorical|  3     | Availability of Exploit (1=Yes, 0=No, NA).| 
| SRC-3 | Price 0day |  Categorical       |  4     | Known or Estimated 0-day Price of the Exploit.| 
| SRC-3 | Price Today |  Categorical      |  4     | Known or Estimated Price of the Exploit as of Today (Daily Updated).| 
| SRC-3 | EPSS Score |  Categorical       |  536     | Current Value of the Exploit Prediction Scoring System.| 
| SRC-3 | EPSS Percentile |  Categorical  |  653     | Percentile of CVE within Current EPSS.| 
| SRC-3| Countermeasure| Categorical      |  4     | Generic Remediation Level Description.| 
| SRC-3| Countermeasure Name| Categorical |  8     | Name of the Suggested Countermeasure.| 
| SRC-3 | Upgrade Version |  Categorical  |  132     | First Known Unaffected Version(s).| 
| SRC-4 | Tweets |  Categorical           |  54     | Tweet Count of a CVE.| 
| SRC-4 | First Tweet Date |  Date        |  165     | Timestamp of the First Tweet.| 
| SRC-4 | Retweet Count |  Categorical    |  44     | Retweets Average.| 
| SRC-4 | Reply Count |  Categorical      |  21     | Reply-Count Average.| 
| SRC-4 | Like Count |  Categorical       |  47     | Like-Count Average.| 
| SRC-4 | Quote Count |  Categorical      |  19     | Quote-Count Average.| 
| SRC-4 | Impression Count |  Categorical |  18     | Impression-Count Average.| 


The Time2Fix dataset can be found in the following [link](https://github.com/criveraalvarez/Time2FixPredictionDS/blob/d09732f25a0af5fabf46323e779902a0b72ca28c/Time2Fix_DS_V1.xlsx).

**Datasets inquiries**
Please submit any inquiry related to the dataset to:
- [Dr. Carlos Rivera - UNSW](mailto:c.rivera_alvarez@unswalumni.com),
- [Dr. Arash Shaghaghi - UNSW](mailto:a.shaghaghi@unsw.edu.au), 
- [Professor Salil Kanhere - UNSW](mailto:salil.kanhere@unsw.edu.au)
