# Apache JMeter Notes

## 1. What is JMeter?

**Apache JMeter** is an open-source **load and performance testing tool** developed by the Apache Software Foundation.

It is mainly used to test the performance of applications by simulating multiple users sending requests to a server/application.

### JMeter can be used for:

* Load Testing
* Performance Testing
* Stress Testing
* Spike Testing
* API Testing
* Web Application Testing
* Database Testing
* FTP testing, etc.

**Example:**
If 1,000 users access a login API at the same time, JMeter can simulate those users and measure how the application behaves.

---

# 2. How to Download JMeter?

JMeter is available from the official Apache JMeter website.

[Apache JMeter Official Website](https://jmeter.apache.org/?utm_source=chatgpt.com)

### Steps:

1. Open the Apache JMeter website.
2. Go to **Download Releases**.
3. Download the **Binary** distribution.
4. Extract the downloaded `.zip` file.
5. JMeter does not require a traditional installation.

### Important prerequisite

JMeter requires **Java**.

Check Java:

```bash
java -version
```

Then start JMeter:

**Windows:**

```text
apache-jmeter-x.x/bin/jmeter.bat
```

**Linux:**

```bash
./jmeter
```

---

# 3. What is a Test Plan?

A **Test Plan** is the root element of a JMeter test.

It contains all the components required to execute a performance test.

For example:

```text
Test Plan
   |
   └── Thread Group
          |
          ├── HTTP Request
          └── Listener
```

A Test Plan can contain:

* Thread Groups
* Samplers
* Controllers
* Listeners
* Config Elements
* Assertions
* Timers, etc.

---

# 4. What is a Thread Group?

A **Thread Group** represents a group of virtual users in JMeter.

Each **thread = one virtual user**.

For example:

```text
Number of Threads = 100
```

means JMeter will simulate **100 virtual users**.

### How to add Thread Group?

Right-click:

```text
Test Plan
   ↓
Add
   ↓
Threads (Users)
   ↓
Thread Group
```

---

# 5. Number of Threads

**Number of Threads** determines the number of virtual users JMeter will simulate.

Example:

```text
Number of Threads = 10
```

JMeter simulates:

```text
10 virtual users
```

If:

```text
Number of Threads = 100
```

JMeter simulates:

```text
100 virtual users
```

### Important

Thread ≠ physical computer user.

A JMeter thread is a **virtual user executing the configured test flow**.

---

# 6. Ramp-Up Period

**Ramp-Up Period** determines how long JMeter takes to start all the configured threads.

### Example:

```text
Number of Threads = 10
Ramp-Up Period = 20 seconds
```

JMeter will start the 10 users gradually over approximately 20 seconds.

Conceptually:

```text
0 sec     → User 1
2 sec     → User 2
4 sec     → User 3
...
20 sec    → User 10
```

A simple formula:

```text
Approximate delay between users
= Ramp-Up Period / Number of Threads
```

Example:

```text
20 / 10 = 2 seconds
```

So users are started approximately every 2 seconds.

### Why use Ramp-Up?

Instead of suddenly sending:

```text
1000 users → Server
```

you can gradually increase the load:

```text
10 → 20 → 50 → 100 → ... → 1000 users
```

This helps simulate a more controlled load pattern.

---

# 7. Loop Count

**Loop Count** determines how many times each thread executes the configured test scenario.

Example:

```text
Threads = 5
Loop Count = 3
```

Each of the 5 users executes the scenario 3 times.

Therefore:

```text
5 users × 3 loops = 15 executions
```

Example:

```text
Thread 1 → Request → Request → Request
Thread 2 → Request → Request → Request
Thread 3 → Request → Request → Request
Thread 4 → Request → Request → Request
Thread 5 → Request → Request → Request
```

### Forever

If **Forever** is selected, the thread continues executing until the test is stopped or another configured condition ends it.

---

# 8. How to Add an HTTP Request

An **HTTP Request** is a JMeter **Sampler** used to send HTTP/HTTPS requests to a web server or API.

### Step 1 — Create Thread Group

Right-click:

```text
Test Plan
 → Add
 → Threads (Users)
 → Thread Group
```

Configure:

```text
Number of Threads: 10
Ramp-Up Period: 10
Loop Count: 1
```

---

### Step 2 — Add HTTP Request

Right-click **Thread Group**:

```text
Add
 → Sampler
 → HTTP Request
```

---

### Step 3 — Configure the request

Suppose you want to test:

```text
https://example.com/login
```

Configure:

**Protocol:**

```text
https
```

**Server Name or IP:**

```text
example.com
```

**Port Number:**

```text
443
```

**Method:**

```text
GET
```

**Path:**

```text
/login
```

So JMeter constructs:

```text
https://example.com/login
```

---

### For POST request

For example:

```text
POST /login
```

You can configure:

```text
Method: POST
Path: /login
```

Then add request parameters or a request body depending on the API.

For JSON APIs, you may also need an **HTTP Header Manager**, for example:

```text
Content-Type: application/json
```

---

# 9. What is a Listener?

A **Listener** is a JMeter component used to **collect, display, and analyze test results**.

Listeners help you understand things such as:

* Response time
* Number of requests
* Errors
* Throughput
* Response data
* Success/failure

Common listeners include:

```text
View Results Tree
Summary Report
Aggregate Report
Response Time Graph
```

---

# 10. What is View Results Tree?

**View Results Tree** is a Listener that allows you to inspect individual request results.

It can show:

* Request
* Response
* Response code
* Response headers
* Response body
* Request timing
* Success/failure

### How to add it?

Right-click **Thread Group**:

```text
Add
 → Listener
 → View Results Tree
```

Then execute your test.

You can select an individual request and inspect its response.

For example:

```text
HTTP Request
     ↓
HTTP 200 OK
     ↓
Response Body
```

### Important interview point

**View Results Tree is useful for debugging and functional verification, but it should generally be avoided during large load tests because storing/displaying every response consumes resources.**

For larger performance tests, lightweight listeners such as **Summary Report/Aggregate Report**, or preferably non-GUI execution with result files, are more appropriate.

---

# 11. Should I Save the Test Plan Before Executing?

**Yes. It is good practice to save the Test Plan before running the test.**

JMeter test plans are normally saved as:

```text
.jmx
```

Example:

```text
Login_Performance_Test.jmx
```

### Save:

```text
File
 → Save Test Plan As
```

or use:

```text
Ctrl + S
```

### Why save it?

Because the `.jmx` file contains your test configuration, such as:

```text
Test Plan
   ↓
Thread Group
   ↓
HTTP Request
   ↓
Listeners
   ↓
Assertions
   ↓
Timers
   ↓
Other configurations
```

You can later reopen the `.jmx` file and execute or modify the test.

---

# 12. Complete Basic JMeter Flow

For your notes, remember this structure:

```text
                    TEST PLAN
                        |
                        ↓
                  THREAD GROUP
                        |
          ┌─────────────┴─────────────┐
          ↓                           ↓
   Virtual Users                Test Configuration
   - Threads                    - Ramp-Up
   - Loop Count                 - Loop Count
          |
          ↓
       SAMPLER
    HTTP Request
          |
          ↓
   CONFIG ELEMENTS
   Header Manager
   CSV Data Set Config
          |
          ↓
       ASSERTION
   Validate Response
          |
          ↓
       LISTENER
   View Results Tree
   Aggregate Report
```

### Simple example

If you configure:

```text
Threads       = 10
Ramp-Up       = 20 seconds
Loop Count    = 2
```

and have:

```text
HTTP Request → GET /login
```

then each virtual user performs the request twice:

```text
10 users × 2 loops
= 20 request executions
```

The ramp-up controls **how quickly those 10 users are started**.

---

## Quick Interview Revision

| Term                  | Meaning                                        |
| --------------------- | ---------------------------------------------- |
| **JMeter**            | Open-source performance/load testing tool      |
| **Test Plan**         | Root container for the test                    |
| **Thread Group**      | Defines virtual users and execution behavior   |
| **Thread**            | One virtual user                               |
| **Number of Threads** | Number of virtual users                        |
| **Ramp-Up Period**    | Time taken to start the configured users       |
| **Loop Count**        | Number of times each user repeats the scenario |
| **Sampler**           | Sends requests, e.g. HTTP Request              |
| **HTTP Request**      | Sends HTTP/HTTPS request to the target         |
| **Listener**          | Displays/collects test results                 |
| **View Results Tree** | Inspects individual request/response results   |
| **`.jmx`**            | JMeter Test Plan file                          |

**One important distinction to remember:**
**Thread Group → controls users; Sampler → sends requests; Listener → shows results.**

Yes. Since this is **your own website**, a small controlled JMeter test is a good way to learn load testing without generating unnecessary traffic.

For your notes, you can use this example:

# Example: Load Testing My Website with JMeter

**Target website:** `https://www.shuvradipchakraborty.me/`

> Start with a very small load such as **5–10 virtual users**. Do not immediately test with hundreds or thousands of users, especially on a personal/low-cost hosting environment.

## 1. Create a Test Plan

Open JMeter:

```text
Test Plan
```

Rename it:

```text
Shuvradipchakraborty Website - Load Test
```

---

## 2. Add Thread Group

Right-click:

```text
Test Plan
   ↓
Add
   ↓
Threads (Users)
   ↓
Thread Group
```

Configure a conservative test:

```text
Number of Threads (Users): 5
Ramp-Up Period:             10 seconds
Loop Count:                 2
```

### What does this mean?

**5 Threads**

→ JMeter simulates 5 virtual users.

**10-second Ramp-Up**

→ JMeter gradually starts those 5 users over approximately 10 seconds.

**Loop Count = 2**

→ Each virtual user visits the configured page twice.

Therefore, the basic request count is approximately:

```text
5 users × 2 loops = 10 requests
```

---

# 3. Add HTTP Request

Right-click:

```text
Thread Group
   ↓
Add
   ↓
Sampler
   ↓
HTTP Request
```

Configure:

### Name

```text
Open Personal Website
```

### Protocol

```text
https
```

### Server Name or IP

```text
www.shuvradipchakraborty.me
```

### Port

Leave it blank because HTTPS normally uses the default HTTPS port.

### Method

```text
GET
```

### Path

```text
/
```

So the request becomes:

```text
GET https://www.shuvradipchakraborty.me/
```

---

# 4. Add a Listener

For learning/debugging:

```text
Right-click Thread Group
   ↓
Add
   ↓
Listener
   ↓
View Results Tree
```

This allows you to inspect individual requests.

You can see things such as:

```text
Request
Response Code
Response Headers
Response Body
Response Time
```

---

# 5. Save the Test Plan

Before executing:

```text
File
   ↓
Save Test Plan As
```

For example:

```text
Shuvradipchakraborty_Website_Load_Test.jmx
```

The `.jmx` file is your JMeter test plan.

---

# 6. Run the Test

Click the **Start ▶** button.

Your basic test is:

```text
                    Test Plan
                        |
                        ↓
                  Thread Group
                        |
             5 Virtual Users
                        |
                10 sec Ramp-Up
                        |
                  2 Loops/User
                        |
                        ↓
                HTTP Request
                        |
                        ↓
       GET / on your website
                        |
                        ↓
              View Results Tree
```

---

# 7. What should you check?

After the test, look at the response.

For example:

```text
HTTP Request
     |
     ├── Response Code: 200
     ├── Response Time: XXX ms
     └── Success: True
```

A successful `GET` request commonly returns:

```text
200 OK
```

But **don't use HTTP 200 alone as your performance conclusion**. You should also examine response time, throughput, errors, and behavior as load increases.

---

# 8. Then Increase the Load Gradually

Once the 5-user test works correctly, you can perform controlled steps.

### Test 1 — Baseline

```text
Users:       1
Ramp-Up:     1 sec
Loop Count:  1
```

### Test 2 — Small Load

```text
Users:       5
Ramp-Up:     10 sec
Loop Count:  2
```

### Test 3 — Moderate Load

```text
Users:       10
Ramp-Up:     20 sec
Loop Count:  2
```

### Test 4 — Higher Load

Only if your hosting/resources can safely handle it:

```text
Users:       20
Ramp-Up:     40 sec
Loop Count:  2
```

The important concept is:

```text
1 user
   ↓
5 users
   ↓
10 users
   ↓
20 users
   ↓
Observe performance
```

Rather than immediately doing:

```text
1000 users → website
```

---

# 9. What metrics should I record?

For your notes, record:

| Metric                    | Meaning                                  |
| ------------------------- | ---------------------------------------- |
| **Response Time**         | How long the server takes to respond     |
| **Throughput**            | Requests handled over time               |
| **Error %**               | Percentage of failed requests            |
| **HTTP Response Code**    | For example, 200, 404, 500               |
| **Average Response Time** | Average request processing time          |
| **Min/Max Response Time** | Fastest and slowest responses            |
| **Latency**               | Time before the response begins arriving |

For a performance test, don't focus only on the **View Results Tree**. For larger tests, use summary/aggregate results and preferably run JMeter in **non-GUI mode**.

---

## 10. A Good Beginner Test Configuration

For your website, I'd write the first exercise in your notes like this:

```text
Test Name:
Shuvradipchakraborty.me Basic Load Test

Target:
https://www.shuvradipchakraborty.me/

Thread Group:
    Threads: 5
    Ramp-Up: 10 seconds
    Loop Count: 2

Sampler:
    HTTP Request
    Method: GET
    Protocol: HTTPS
    Server: www.shuvradipchakraborty.me
    Path: /

Listener:
    View Results Tree
```

### Expected flow

```text
5 Virtual Users
       ↓
Gradually started over 10 seconds
       ↓
Each user sends GET /
       ↓
Each user repeats 2 times
       ↓
Approximately 10 HTTP requests
       ↓
JMeter collects the results
```

**One caution:** I couldn't directly fetch your site from my browsing environment because the request was redirected to an anti-bot/Google interstitial, so I haven't assumed anything about your site's hosting, backend, or capacity. 

For a real test, start with the small configuration above and watch your hosting/server monitoring while it runs.
