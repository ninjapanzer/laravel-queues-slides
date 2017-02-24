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

- Creates application environment in about 10 minutes
- Bulletproof
- Built in Continuous Delivery with rollback

---

## Infrastructure Overview

- `docker-compose.yml` [https://github.com/ninjapanzer/laravel-queues-docker/blob/master/docker-compose.yml](https://github.com/ninjapanzer/laravel-queues-docker/blob/master/docker-compose.yml)
- Laravel queue.php [https://github.com/ninjapanzer/laravel-queues-project/blob/master/config/queue.php#L44-L49](https://github.com/ninjapanzer/laravel-queues-project/blob/master/config/queue.php#L44-L49)
- `.env` changes [https://github.com/ninjapanzer/laravel-queues-project/blob/master/.env#L18-L19](https://github.com/ninjapanzer/laravel-queues-project/blob/master/.env#L18-L19)

---

## How is a job designed

- Jobs are POPOs that define how an async process should be resolved
- They should be atomic and additional order independent
- Job state should be scalar values (It will eventually be serialized)
- There is no receiver (They do their work and go away)

---

## Example Job
[https://github.com/ninjapanzer/laravel-queues-project/blob/master/app/Jobs/LongTask.php](https://github.com/ninjapanzer/laravel-queues-project/blob/master/app/Jobs/LongTask.php)
<pre>
class LongTask implements ShouldQueue
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels;
    public function __construct(){}
    public function handle()
    {
        sleep(10);
    }
}
</pre>

---

## Dispatch

- For Laravel the job queuing is called dispatching
- By default jobs can be queued immediately or delayed to start later
- Laravel provides a global helper called `dispatch` which accepts an instance of a job

---

## What happens next

1. The job is serialized
2. Send over the network to beanstalkd which places it in a pipe(queue)
3. Worker is started
4. Worker reserves a job from the pipe
5. Worker deserializes the job and calls re-instantiates the POPO

---

## Lets Watch

- [localhost](localhost)
- [localhost:8081](localhost:8081)


