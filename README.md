# bull-board 🎯

Bull Dashboard is a UI built on top of [Bull](https://github.com/OptimalBits/bull) to help you visualize your queues and their jobs.

<p align="center">
  <a href="https://github.com/vcapretz/bull-board/blob/master/LICENSE">
    <img alt="licence" src="https://img.shields.io/npm/l/dockest.svg?style=flat">
  </a>
  <a href="https://snyk.io/test/github/vcapretz/bull-board">
    <img alt="snyk" src="https://snyk.io/test/github/vcapretz/bull-board/badge.svg">
  </a>
<p>

## Starting

To add it to your project start by adding the library to your dependencies list:

```sh
yarn add bull-board
```

Or

```sh
npm i bull-board
```

## Hello world

Remember that it depends on Redis as well, so the first step is to configure all of your queues:

```js
const { createQueues } = require('bull-board')

const redisConfig = {
  redis: {
    port: process.env.REDIS_PORT,
    host: process.env.REDIS_HOST,
    password: process.env.REDIS_PASSWORD,
  },
}

const queues = createQueues(redisConfig)
```

And then you can setup how your queues will work:

```js
const helloQueue = queues.add('helloQueue') // adds a queue
const helloQueueWithOpts = queues.add('helloQueue', {
    prefix: 'hello',
}) // adds a queue as QueueOptions https://github.com/OptimalBits/bull/blob/master/REFERENCE.md#queue

// defines how the queue works
helloQueue.process(async job => {
  console.log(`Hello ${job.data.hello}`)
})

helloQueue.add({ hello: 'world' }) // adds a job to the queue
```

And finally, add `UI` to your middlewares (this can be set up using an admin endpoint with some authentication method):

```js
const app = require('express')()
const { UI } = require('bull-board')

app.use('/admin/queues', UI)

// other configurations for your server
```

## Developing

To try it out locally you can clone this repo and run:

```sh
yarn && yarn start:example
```

Just make sure you spin up a Redis instance locally (default port is 6379).

## Further ref

For further ref, please check [Bull's docs](https://optimalbits.github.io/bull/). Apart from the way you configure and start your UI, this library doesn't hijack Bull's way of working.

If you want to learn more about queues and Redis: https://redis.io/

## UI

![UI](https://raw.githubusercontent.com/vcapretz/bull-board/master/shot.png)

### Acknowledgements ❤️

- [Juan](https://github.com/joaomilho) for building the first version of this library
