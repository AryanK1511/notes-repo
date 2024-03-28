# Redis Essential Training

## Installing Redis

1. Install it from the [website](https://redis.io/docs/install/install-redis/).
2. Use `redis-server` to run the server.
3. Use `redis-cli` to run the redis command line interface.
    1. **Redis Server**: All command logic runs from here.
    2. **Redis Client**: Only useful to test commands on the server. You can have as many instances of this running as you want.
        - You can use only one command per line
        - The Client will give you hints on the command you are trying to use

## Basics of Redis

It is a key-value, in memory, noSQL database. It allows you to store hashmaps in memory without any database schema. 
- No Complex data models
- No hard drives
- No delays in data access
- Writing and reading is really fast as it all happens in the memory
- Use redis to store info which requires quick IO
- In-memory data can always be lost
- Usually used to store data to improve performance of your overall system and not as the main DB

## Data Structures

- Strings
- Lists
- Sets
- Hashes
- Sorted sets
- Bitmaps
- HyperLogLogs
- Streams
- Geospatial Indices