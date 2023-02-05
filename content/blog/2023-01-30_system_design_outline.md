+++
title = "System Design template"

[taxonomies]
tags = ["system_design", "interview", "architecture"]
+++

When dealing with System Design it is important to have some outline.
The one described in the article is commonly used in FAANG interviews. 
Although it is useful to use the same approach when writing HLDs for 
your new features on the daily job basis.

# Functional requirements

This part defines the feature to be implemented. When working on a project
those will become your tasks. On the interview it is trickier, as 
interviewer may want to see your capability to decompose the problem and 
ask clarifying questions. 

Nice to ask questions:
- What are the features?
    - For easier definitions you could use `UserStory` approach.  
      Template:   
      As a `<role>`, I want `<goal>` so that `<reason>`.  
      Example:   
      As a User I want to have form where I can upload video stories to share it with my friends.
- What problem does it solve?
    - Are there any existing solutions?
- Who is the target audience?
- How is it going to be monetised?
- Business KPIs to measure success

# Non-Functional requirements

Then, when you know what to be developed, you need to specify the `*-ilites` of the system.  
This step will help you to make decisions on how to design the system knowing its expected
scale, uptime, data storage and security requirements.

- Durability (can data be lost?)
    - as a User I want my data to not be lost, so I could access it later.

- Fault-tolerant (what if something goes wrong/down?)
    - as a CTO I want the system to be resilient such that users will have the best experience with a system and
      for system to be self recoverable

- Scalable (is it possible to scale horizontally? what are the benefits of vertical scaling?)
    - as a CTO I want the system to be easy to scale such that in case of user increase, system can be scaled
      without additional issues.

- Highly available (how to set up? what are the properties within CAP theorem)
    - do not forget the desired SLA in percentiles
    - as a user I want system to be available 99.99 percent of time
    - for distributed architecture it should affect
        - type of storage and what kind of guarantees it can give

- Modularity
    - Property of the system such that some functionality can easily be used without affecting others.
    - It can be a significant performance improvement for example to separate services by read and write
      applications improving overall availability and scalability of the system

- Responsive | Low latency
    - As a user I want it to respond very fast so I could check stories of my friends without waiting.

- Security [(Stride)](https://en.wikipedia.org/wiki/STRIDE_(security))
    - Spoofing (Authenticity)
        - Can someone pretend to be someone else?

    - Tampering (Integrity)
        - Can I modify the request parameters to get different behavior?

    - Repudiation (Non-reputability)
      - Can someone deny the performed this action?

    - Information disclosure (Confidentiality)
        - Users obtaining more information than they are allowed to. What information could get exposed to this
          asset, what's contained in database?

    - DoS (Availability)

        - Some actions could result in DoS, through spamming actions that require a lot of resources or result
          in quota limits.

    - Elevation of Privilege (Authorization)

        - Users obtaining more permissions than they are allowed to. Typical way to resolve is to use as little
          permissions as possible, to minimise the attack vector

# Capacity Estimation

This step helps to answer questions:

* what are the storage requirements?
* what is the expected volume of data?
* how many cache nodes to use?
* can be used for cost counting

It is better to define in the following order:
- Load

  - `QPS` - queries per second, as if how many times user want to ask system a specific question
or to do specific task.

    It is calculated using the next formula:  
    `QPS` = `DAU` * `AVG_QUERIES_PER_DAY`

  - `RPS` - requests per second, referring to actual count of http requests.

Example, describing the difference between `QPS` and `RPS`:
if we take google search as our target system to calculate QPS and RPS then
`QPS` would be a measure of how many searches are done per second,
meaning that our query is the search.
`RPS` would be a measure of requests done to the system, and it would also include
suggestion, static asset and other requests, thus it is always greater or equal to `QPS`.

- Storage

  - how your model looks like? calculate the size in bytes.
  - how much storage will it take?
      - for one day
      - for one month
      - for 5 years

- Cache requirements

  - how much GB per day?
  - how many machines needed to store your cache?
      - for example, you have 3TB of cache   
        and single machine is able to store 150GB  
        then you would need 20 servers to handle the load

# High Level Design

Here you should create a diagram of the system noting the functional and non-functional requirements specified
beforehand.

Do not forget to note places where you

- sacrifice consistency over availability;
- show the weakest spots and how to resolve them;

# API Design

Describe the APIs you are going to design.

- What protocol are you using?
    - HTTP | REST
    - RPC | gRPC
- Sync or async? long polling vs polling?

# Numbers all developers should know

## Power of two
---

| POWER  | APPROXIMATE VALUE | FULL NAME  | SHORT NAME  |
|:------:|:------------------|:-----------|:-----------:|
|   10   | 1 thousand        | 1 Kilobyte |    1 KB     |
|   20   | 1 million         | 1 Megabyte |    1 MB     |
|   30   | 1 billion         | 1 Gigabyte |    1 GB     |
|   40   | 1 trillion        | 1 Terabyte |    1 TB     |
|   50   | 1 quadrillion     | 1 Petabyte |    1 PB     |

---

## Latency numbers
---

| OPERATION NAME                                | TIME                         |
|:----------------------------------------------|:-----------------------------|
| L1 cache reference                            | `0.5 ns`                     |
| Branch mis-predict                            | `5 ns`                       |
| L2 cache reference                            | `7 ns`                       |
| Mutex lock/unlock                             | `100 ns`                     |
| Main memory reference                         | `100 ns`                     |
| Compress 1K bytes with Zippy                  | `10_000 ns`  = `10 µs`       |
| Send 2K bytes over 1 Gbps network             | `20_000 ns` = `20 µs`        |
| Read 1MB sequentially from memory             | `250_000 ns` = `250 µs`      |
| Roundtrip within the same datacenter          | `500_000 ns` = `500 µs`      |
| Disk seek                                     | `10_000_000 ns` = `10ms`     |
| Read 1MB sequentially from the network        | `10_000_000 ns` = `10 ms`    |
| Read 1MB sequentially from disk               | `30_000_000 ns` = `30 ms`    |
| Send packet CA (California) ->Netherlands->CA | `150_000_000 ns` = `150 ms`  |

---
Taken from [Calculator per year by google developer](https://colin-scott.github.io/personal_website/research/interactive_latency.html)

## Availability numbers

<div class="overflow-x-auto">

---
| Availability % | Downtime per day | Downtime per week | Downtime per month | Downtime per year |
|:---------------|:-----------------|:------------------|:-------------------|:------------------|
| 99%            | 14.40 minutes    | 1.68 hours        | 7.31 hours         | 3.65 days         |
| 99.99%         | 8.64 seconds     | 1.01 minutes      | 4.38 minutes       | 52.6 minutes      |
| 99.999%        | 864 ms           | 6.05 seconds      | 26.30 seconds      | 5.26 minutes      |
| 99.9999%       | 86.40 ms         | 604.8 ms          | 2.64 seconds       | 31.56 seconds     |
---

</div>

# Tips

Back-of-the-envelope estimation is all about the process. Solving the problem is more important than obtaining
results. Interviewers may test your problem-solving skills. Here are a few tips to follow:

* Rounding and Approximation. It is difficult to perform complicated math operations during the interview. For
  example, what is the result of “99987 / 9.1”? There is no need to spend valuable time to solve complicated math
  problems. Precision is not expected. Use round numbers and approximation to your advantage. The division question
  can be simplified as follows: “100,000 / 10”.
* Write down your assumptions. It is a good idea to write down your assumptions to be referenced later.
* Label your units. When you write down “5”, does it mean 5 KB or 5 MB? You might confuse yourself with this. Write
  down the units because “5 MB” helps to remove ambiguity.
* Commonly asked back-of-the-envelope estimations: QPS, peak QPS, storage, cache, number of servers, etc. You can
  practice these calculations when preparing for an interview. Practice makes perfect.

# References

- <https://itsallbinary.com/system-design-back-of-envelop-calculations-for-storage-size-bandwidth-traffic-etc-estimates>
- <https://aaronice.gitbook.io/system-design/system-design-systematic-approach>
- <https://gist.github.com/vasanthk/485d1c25737e8e72759f>
- <https://matthewdbill.medium.com/back-of-envelope-calculations-cheat-sheet-d6758d276b05>
- !!! <https://blog.junianto.com/2022/06/28/back-of-the-envelope-estimation/>
    - <http://highscalability.com/blog/2011/1/26/google-pro-tip-use-back-of-the-envelope-calculations-to-choo.html>
    - <https://github.com/donnemartin/system-design-primer>
    - <https://colin-scott.github.io/personal_website/research/interactive_latency.html>
    - <https://aws.amazon.com/compute/sla/>
    - <https://cloud.google.com/compute/sla>
    - <https://azure.microsoft.com/en-us/support/legal/sla/summary/>