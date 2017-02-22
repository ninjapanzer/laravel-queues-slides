## Laravel Queues and Beanstalkd (and Docker)
 
Paul Scarrone (@PaulSCoder)

SavvySoftWorks

---

## So... PHP

- Limited memory per process
- Runtime limits
- Making people wait (Not really PHP's issue)

---

## Job Queues

- Split up long running jobs into small atomic processes
- Defer server-client delay
- Balance server load

---

## Why Beanstalkd?

- Lightweight
- Resilient
- Written in C

---

## Other Queues

- Amazon SQS
- Redis
- Relational Database

---

## Why Beanstalkd!

- Maintains an generic interface for jobs
- Uses a single file binlog for storage
- Designed specifically to offload long running processes for web-servers [Facebook Causes](http://kr.github.io/beanstalkd/)

---

## Where does (and Docker) come in?
(In No Particular Order)

1. I am lazy
2. I don't want to know how to install stuff
3. I want it to be bullet proof

---

## Docker Example

[Docker Continuous Delivery Example](https://github.com/SavvySoftWorksLLC/continuous-laravel-docker)

- Runs infrastructure you need in about 10 minutes
- Bulletproof
- Built in Continuous Delivery with rollback

---

