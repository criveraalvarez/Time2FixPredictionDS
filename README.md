![unsw_0](https://user-images.githubusercontent.com/7439960/197022279-cfdf58ea-01ca-4c1e-854e-99776012a0bd.png) 
# Time2Fix Prediction Dataset

**Authors**  
  Carlos A. Rivera A.(1), Arash Shaghaghi1(1), Gustavo Batista(1), and Salil S. Kanhere(1)  
   1. The University of New South Wales (UNSW) Sydney, Australia.  
  
  
**Abstract**  
In our cutting-edge IoT-focused research initiatives, we have made substantial strides in developing and deploying two sophisticated machine learning algorithms. The first algorithm is primarily dedicated to predicting vulnerabilities inherent in IoT devices, while the second algorithm is focused on forecasting attack patterns that stem from weaknesses in susceptible IoT devices. The successful utilization of these algorithms offers invaluable assistance to ICT administrators at the enterprise level, enabling them to effectively manage IoT devices within their organisation by proactively addressing vulnerabilities associated with high-risk weaknesses and high potential of exploiting attack patterns. Our rigorous comparative analysis involved assessing the performance metrics of our implementations against similar policy models, confirming the reliability and trustworthiness of the results for deployment in real-world production environments. We share next the two datasets we used in our research.

**Keywords**  
CyberSecurity in IoT | Common Weakness Enumeration (CWE) | Common Attack Pattern Enumeration and Classification (CAPEC)


**The Datasets**  
We aim to analyse text and, through ML models, predict the weakness(es) for any IoT device. We define hence the necesity of a dataset with mostly text as its content, and divided into two parts. The first, a text sequence (the input) and the classes to predict (the output). The output would contain a finite list of weaknesses(CWEs). 
Searching for possible sources, we used the dataset from [Rivera et al.](https://doi.org/10.1007/978-3-030-94822-1\_7) and added technical information (from Zoomeye) to form our dataset-0 (Table-1). Analysing this dataset, we found two problems; First, the information is organised in features rather than just text; Secondly, the classes predicted are risk labels(Low, Medium, High, Critical). 

**Table 1. Dataset-0 features.**

Description of features of dataset from [Rivera et al.](https://doi.org/10.1007/978-3-030-94822-1\_7)(SRC-1), ZoomEye (RC-2) and The NVD (SRC-3)
|Source |Feature Name |Data Type |Unique Values|Details|
| --- | --- | --- | --- | --- |
|SRC-1  |Brand        |Categorical|            129     |Name of the device reported on the CVE.\\
|SRC-1  |Product Type |Categorical|             71     |Phrase describing the product.\\
|SRC-1  |Category     |Categorical|              5     |SmartHome, Medical, Wearable, Telecomm, and Other.\\
|SRC-2  |Model        |Categorical|            Infinite |Product model identifier.\\
|SRC-2  |Operating System|    Categorical|     Infinite |Operating System name identifier.\\
|SRC-2  |Operating System Version|Categorical| Infinite |Operating System version identifier.\\
|SRC-2  |Ports       |Categorical|             65535    |Numeric identifier of the port(s) detected open.\\
|SRC-2  |Services    |Categorical|             [Infinite](https://www.rfc-editor.org/rfc/rfc6335) |Name of service associated to each open port.\\
|SRC-3  |Vulnerability Description|Categorical|Infinite |Text of each vulnerability description.\\
|SRC-3  |Weakness Description|Categorical     |926       |Text of each weakness description.\\
|SRC-3  |Weakness ID   |Categorical           |926       |Identifier code assigned to each weakness. E.g. CWE-001\\ 
