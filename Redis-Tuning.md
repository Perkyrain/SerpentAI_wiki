## Max Memory
> WARNING: not setting maxmemory will cause Redis to terminate with an out-of-memory exception if the heap limit is reached.

You should always define the max memory that Redis can take and define what happens when it is reached because it will have very different results. This [site](https://scaleyourcode.com/blog/article/15) has some insights.

## Persistence
From the Redis website:
> RDB needs to fork() often in order to persist on disk using a child process. Fork() can be time consuming if the dataset is big, and may result in Redis to stop serving clients for some millisecond or even for one second if the dataset is very big and the CPU performance not great. AOF also needs to fork() but you can tune how often you want to rewrite your logs without any trade-off on durability. [Link](https://redis.io/topics/persistence)

As we don't need persistence to train SerpentAI, this feature _can_ be turned off.

To turn off persistence, replace `# persistence-available [(yes)|no]` to `persistence-available no` in the redis configuration file.