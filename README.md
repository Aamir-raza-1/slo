# slo
## Title of the SLO Document

This document describes the SLO for `chatbot service`.

|Status        |Published |
|--------------|:--------:|
|Author        |   Aamir Raza       |
|Date          |   2021-04-26      |
|Reviewers     |          |
|Approvers     |          |
|Approval date |          |
|Revisit date  |          |

## Service Overview
Transform chatbot based user and cosumer experience that is integrated with Facebook and Line Messenger.Platform core services are 

1. Chatbot management web application 
2. Message sending and receiving service to messenger 
3. Group of services that cut out common processing of prior services.

Users interacts with application through  messenger,its inflows and utterances are sent back to messagig service .Serverless pipeline is in built between user and messaging service.Serverless stacks JSON data and returns HTTP response immediately.
Web application is  for user management ,CRUD  of chatbot utterance content and DB linkage with client.It faces cloud load balancer and fastly for static file delivery.
gRPC based group of services for providing common functionlity such as narrowing down of users.
Memory store  is used for scenarios distribution to users.
All  data is persisted into single database for CRUD ops.


The SLO is uses a four week rolling window.<br>
Each objective has a separate error budget <br>
Formula = 100% minus (-) the goal for that objective. <br>

SLI's capture ratio of good events to total events <br>
Error budget gives number of allowed bad events. <br>
Error rate is the ratio of bad events to total events <br>

## SLIs and SLOs


                    
|Category               |SLI                                                              | SLO                        | Error budget | Error rate |Source|
|:---:|------------------------------------------------------------------------|:---:|---|---|---|
| **Request Driven API**                      |                                                                   |                               | Total no. of requests are **1,000,000** than value of error budget is |||
|[Availability](https://linkedin.github.io/school-of-sre/systems_design/availability)|_Any HTTP status code other than 500-599 is considered successful_|    |   |||
|             |  Proportion of successful http requests / total http requests     |  **97% success** |  **3% = 30000** errors             | 3% ||
|[Latency](https://sre.google/sre-book/monitoring-distributed-systems/#latency)  |  Proportion of fast reqs <400 ms / total no. of reqs  |**90% reqs <400ms**| **10% = 1,000,00** reqs<400ms | 10% ||
||  Proportion of fast reqs <800 ms / total no. of reqs|**97% reqs <800ms**| **3% = 30000** reqs<800ms| 3% ||
||  Proportion of slow reqs <6000 ms / total no. of reqs|**80% reqs <6000ms**| **20% = 2,000,00** reqs<6secs| 20%||
||  Proportion of slow reqs <8000 ms / total no. of reqs|**89% reqs <8000ms**| **11% = 1,100,00** reqs<8secs|11%||
|[Error](https://sre.google/sre-book/monitoring-distributed-systems/#errors)|**Explicit:**   _HTTP 500-599_||||
|| Proportion of errors having status code / total http reqs | **3% error**| **3% = 3,000,0** errors  | 3%||
||**Implicit:**   _HTTP 200 but coupled with wrong content_ |||||
|| Proportion of errors having wrong content / total http reqs | **1% error**| **1,000,0** errors | 1% ||
||**Policy:**  ||||
|| Committed to 1 sec response time but delayed | **3% conflict with defined policy**| **3,000,0** errors| 3% ||
|[Quality](https://sre.google/workbook/implementing-slos/#slis-for-different-types-of-services)|Proportion of successful reqs when cpu overloaded 90 %| **80 % success**|**20% = 2,000,00** errors| 20%||
||Proportion of successful reqs when memory overloaded 90 %| **80% success**| **20% = 2,000,00** errors | 20%||
||Proportion of successful reqs whe datastore is unavailable %| **80% success**| **20% = 2,000,00** errors | 20%||
|**Web server**|||||
| [Availability](https://sre.google/workbook/implementing-slos/#slis-for-different-types-of-services)            |  Proportion of successful web requests / total web requests     |  **99.9% success** | **0.1% = 1000**errors             | 0.1% ||
|[Latency](https://sre.google/sre-book/monitoring-distributed-systems/#latency)  |  Proportion of fast reqs <200 ms / total no. of reqs  |**90% reqs <200ms**| **10% = 1,000,00** reqs<200ms | 10%| |
||  Proportion of fast reqs <1000 ms / total no. of reqs|**99% reqs <1000ms**| **1% = 1,000,0** reqs<1secs| 1% ||
||  Proportion of slow reqs <6000 ms / total no. of reqs|**80% reqs <6000ms**| **20% = 2,000,00** reqs<6secs| 20% ||
||  Proportion of slow reqs <8000 ms / total no. of reqs|**89% reqs <8000ms**| **11% = 1,100,00** reqs<8secs| 11% ||
|**gRPC Server**||||||
|[Availability](https://sre.google/workbook/implementing-slos/#slis-for-different-types-of-services)|Proportion of successful grpc requests/ total grcp requests |**99.99% success**| **0.01% = 100** errors | 0.01%||
|[Latency](https://sre.google/sre-book/monitoring-distributed-systems/#latency)  |  Proportion of fast reqs <200 ms / total no. of reqs  |**90% reqs <200ms**| **10% = 1,000,00** reqs < 200ms| 10% ||
||  Proportion of fast reqs <1000 ms / total no. of reqs|**97% reqs <1000ms**| **3% = 3,000,0** reqs<1secs | 3% ||
||  Proportion of slow reqs <6000 ms / total no. of reqs|**80% reqs <6000ms**| **20% = 2,000,00** reqs<6secs| 20%||
||  Proportion of slow reqs <8000 ms / total no. of reqs|**89% reqs <8000ms**| **11% = 1,100,00** reqs<8secs | 11% ||
|**Pipeline**|||| |
|[Freshness](https://sre.google/workbook/implementing-slos/#slis-for-different-types-of-services)| Proportion of records read from table recently|||||
||_Recently is defined by 1 min to 10 min_|||||
||Use metrics from API and HTTP server|||||
||Count of all data reqs for "api" & "webserver" with 1 min freshness / total no. of data reqs| **90% of reads use data written previous 1 min**| **10% = 1,000,00 reads** use data written more than 1 min | 10% | |
||Count of all data reqs for "api" & "webserver" with 10 min freshness / total no. of data reqs|**99% of reada use data written previous 10 min **| **1% = 1,000,0 reads** use data written more than 10 min| 1% ||
|[Correctness](https://sre.google/workbook/implementing-slos/#slis-for-different-types-of-services)| Proportion of records injected into table by prober|||||
||Result in correct data beingg read <br> Prober should export outcome metric | **99.999% of records injected by prober results in correct output**| |||
|[Completeness](https://sre.google/workbook/implementing-slos/#slis-for-different-types-of-services)| Proportion of hours in which 100% of data processed (no data skipped) <br> count of pipeline runs that procssed 100 percent of records divided by total pipeline runs | **99 % of pipeline runs cover 100% data**| In case of total 1000 pipelines runs <br> **1% = 10 pipelines**    | 1%||



Suggestions:
Overview of monitoring technique and existing infra should also mentioned.<br>
Development technological stack with exact versions and languages should be mentioned <br>  


References:
1. [SLO](https://sre.google/sre-book/service-level-objectives/)
2. [Implementing SLO](https://sre.google/workbook/implementing-slos/)
3. [Sample SLO Document](https://sre.google/workbook/slo-document/)
