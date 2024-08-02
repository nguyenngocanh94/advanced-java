## Interview Question

How to design a high-concurrency system?

## What the Interviewer Is Really Thinking

Honestly, if an interviewer asks you this question, you have to give it your all. Why? Because many companies specifically mention "experience with high-concurrency systems preferred" in their job descriptions.

If you truly have the skills and experience with high-concurrency systems from working at an internet company, getting the offer should be a piece of cake. The interviewer wouldn't ask you this question unless they were clueless.

Let's say you've worked on a high-concurrency system at a well-known e-commerce company with hundreds of millions of users, billions of daily requests, and tens of thousands of concurrent requests during peak hours. In that case, the interviewer would delve into your system architecture: what architecture did you use? How was it deployed? How many machines were deployed? How was caching used? How was MQ used? How was the database used? They would want to understand how you handled high concurrency.

Because anyone who has truly worked on high-concurrency systems knows that system architecture divorced from real-world business scenarios is just theoretical. In a real-world scenario with complex business logic and high concurrency, the system architecture is far more intricate than just using Redis and MQ. Combining a real system architecture with actual business requirements results in a complexity that dwarfs any simplistic notion of "high-concurrency architecture."

If an interviewer asks you how to design a high-concurrency system, chances are they believe you lack practical experience. They've scanned your resume, found it underwhelming, and resorted to this question. Essentially, they are trying to gauge your level of independent research and knowledge accumulation.

Of course, their ideal candidate would be someone with hands-on experience in building high-concurrency systems. However, such individuals are rare and difficult to find. Therefore, the next best option is someone who has invested time and effort in studying the subject. Anything is better than someone clueless!

So, this is your chance to shine and showcase your knowledge of high concurrency!

## Dissecting the Question

To truly understand high concurrency, you need to understand its root cause: Why does it occur? Why is it such a big deal?

Simply put, it all boils down to the fact that systems initially relied heavily on databases. However, databases struggle to handle more than a couple of thousand concurrent requests per second. This limitation explains why many companies with rapidly growing businesses but inadequate technology experience system crashes when faced with sudden surges in traffic.

It's hardly surprising, though. How can a database withstand 5,000, 8,000, or even tens of thousands of concurrent requests per second? It's an impossible feat for most databases, including MySQL.

So why is high concurrency such a big deal? Because the number of internet users continues to skyrocket, resulting in high-concurrency requests for countless apps, websites, and systems. It's not uncommon for these systems to handle thousands of concurrent requests per second during peak hours, let alone events like Singles' Day, where that number can surge to tens or even hundreds of thousands.

So how do we handle such immense concurrency coupled with inherently complex business logic? While the true experts are those who have tackled high concurrency within the context of intricate business systems, let me guide you on how to approach this question:

We can break it down into six key points:

-   **System Splitting**
-   **Caching**
-   **Message Queues (MQ)**
-   **Database Sharding and Partitioning**
-   **Read/Write Splitting**
-   **Elasticsearch**

![high-concurrency-system-design](./images/high-concurrency-system-design.png)

### System Splitting

Split a monolithic system into multiple subsystems, utilizing tools like Dubbo. Instead of relying on a single database, each subsystem connects to its dedicated database, effectively increasing the overall concurrency capacity.

### Caching

Caching is crucial. Most high-concurrency scenarios are read-heavy. Utilize a cache to store a copy of frequently accessed data from the database. Since Redis can effortlessly handle tens of thousands of concurrent requests, offloading read requests to the cache significantly improves performance. Analyze your project to identify read-intensive scenarios where caching can effectively address high concurrency.

### Message Queues (MQ)

MQs are indispensable. Even with caching, scenarios involving high-concurrency writes are inevitable. Imagine a business operation requiring dozens of database interactions (inserts, updates, deletions). This can overwhelm your system. Relying solely on Redis for writes is not feasible; it's a cache with data subject to LRU eviction and lacks robust transaction support. MySQL remains essential in such situations. The solution lies in using MQs. Funnel high-volume write requests into an MQ, allowing them to be processed and written to the database gradually, staying within MySQL's capacity. Identify scenarios in your project where complex write operations can benefit from asynchronous processing using MQs to enhance concurrency. MQs can readily handle tens of thousands of concurrent requests.

### Database Sharding and Partitioning

Even with these measures, the database might still face high-concurrency demands. Sharding and partitioning come to the rescue. Split a single database into multiple databases to distribute the load. Further divide tables into smaller chunks to reduce data size and optimize SQL query performance.

### Read/Write Splitting

Often, databases experience read-heavy workloads. Instead of directing all traffic to a single database, implement a master-slave architecture. The master database handles writes, while slave databases handle reads, effectively separating read and write traffic. Add more slave databases as read traffic increases.

### Elasticsearch

Elasticsearch, or ES, is a distributed search and analytics engine designed for scalability. Its distributed nature enables it to handle high concurrency by simply adding more nodes to the cluster. Leverage ES for simpler queries, aggregations, and full-text search operations, further enhancing your system's ability to cope with concurrent requests.

These six points represent essential considerations when designing high-concurrency systems. Combine your existing knowledge with these concepts, focusing on potential challenges and solutions. This demonstrates a solid understanding of the subject matter.

While comprehending these fundamental architectural concepts and technologies like RocketMQ, Kafka, Redis, and Elasticsearch is commendable, it merely qualifies you as a competent candidate. True expertise lies in practical experience. The ability to analyze, design, and implement high-concurrency architectures within a complex, real-world system with hundreds of thousands of lines of code is invaluable.

In reality, most companies seek individuals who have navigated the intricacies of incorporating high-concurrency principles into existing systems. They need someone who can analyze a complex business system, identify areas requiring sharding, determine appropriate caching strategies, and seamlessly integrate these solutions while maintaining data integrity. It's a challenging endeavor, but successfully accomplishing this feat makes you a highly sought-after asset in the job market.
