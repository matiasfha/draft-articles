# What are Node streams, and how to work with them

Understanding Streams in Node is an essential skill you must learn as a Node developer. 
Node.js is an Asynchronous and Event-Driven model and a Single Threaded engine, meaning that you need to be careful when performing data transfers in I/O operations to avoid blocking processes that can damage the user experience.

This article is focused on explaining and showcasing the use of the Streams API. Streams are a robust implementation that is heavily underutilized.

## What are Streams

Generally, a **stream** is a sequence of data made available over time. Think in the pre-concepts that you already have when you hear the word **Stream**: _A continuous flow of "something"_

This concept has become part of the daily life of many users thanks to streaming services. 

The service sends small pieces of information (video or audio) to the user's device. The device can use these little pieces of information to start playing. The sending and receiving actions in these services and applications are a **continuous flow** of data that allows the user to immediately enjoy the content without waiting for the entire file to be downloaded.

In node js **Streams** is a collection of data but with the main difference when compared with your usual collection of data like arrays. **Streams** are read and processed in small chunks instead of being available all at once.

This feature of processing in small pieces gives significant power to this data structure. You can work with vast amounts of data but handling in small pieces.

> Divide and conquer.

What does this mean for you as a developer? You earn some free advantages by using streams to handle your significant data processing needs.
- Simple and native support for composability.
- Memory Efficiency by reducing Memory Utilization.
- Time Efficiency by not waiting for the complete data to be transmitted to start processing it.

The Stream API is presented in four fundamental types: 
* Readable: an abstraction for a source that can be consumed.
* Writable: an abstraction that defines a destination that can be written.
* Duplex: A combination of a Readable and Writable Stream.
* Transform: An extension of a Duplex stream that can be used to modify the data.

> Keep in mind that all streams are also instances of `EventEmitter`, allowing them to emit events you can listen to perform operations.

## Readable Streams

The primary use case for **Streams** is to consume big loads of data. To do this, let's check a quick example of the use of a Readable stream.

```js
const fs = require('fs')
const fileStream = fs.createReadStream('LargeFile')

// Listen for events
fileStream.on('data', (chunk) => {
    console.log('reading a huge file...')
    processTheData(chunk) // Some function that do something with the data
})

fileStream.on('error', (error) => {
    console.error(error.message)
})

```

The code sample shows you how to read a big file with a stream created from the `fs` module. The Stream emits an event named `data` to execute the callback function whenever a new data chunk is available.

## Writable Streams

On the other side of the spectrum, you'll find the `Writable` streams, which allow you to store or write enormous amounts of data in small pieces.

```js
const fs = require('fs')
const fileStream = fs.createWriteStream('LargeFileDestination')

fileStream.on('error', (error) => {
    console.error(error.message)
})

fileStream.write(The data)

```

You can combine the use of both Streams to, for example, copy a big file into another. How would that look?

```js
const fs = require('fs')
const readStream = fs.createReadStream('LargeFile')
const writeStream = fs.createWriteStream('LargeFileDestination')
// Listen for events
readStream.on('data', (chunk) => {
    writeStream.write(chunk)
})

writeStream.end()

```
## The pipe method

But, there is a flaw in this implementation. Since you have two streams, you need to handle events on both of them, here comes into play another of the features of Stream. The **pipe** method.

```js
readableStream.pipe(writableStream)
```

That simple line of code is passing information from one Stream to another. The pipe method is a way to transport data from a source to an output. 
> You can think of the pipe method as a plumbing pipe that moves water from the water tank to a faucet.

The `pipe` method in node provides a way to read and write data elsewhere without handling each data flow part.

```js
const fs = require('fs')
const readStream = fs.createReadStream('LargeFile')
const writeStream = fs.createWriteStream('LargeFileDestination')

readStream.pipe(writeStream)

writeStream.on('finish', () => {
    console.log('Copy successfully done)
})
```

## Transform Streams

These basic examples are just a way to move data from one end to another, but most of the time, applications need to manipulate the data. This is done by using a transform stream.

We can use the previous example code as a base for a new use case: Uppercase the content of a file.
The idea here is to read from one file, uppercase the content, and write to another file. 

For this, let's implement a transform Stream and use the pipe method to pass the data from the RedableStream into the WritableStream, similar to the previous copy file example.

```js
const { Transform } = require("stream");
const fs = require('fs')

const upperCaseTr = new Transform({
  transform(chunk, encoding, callback) {
    this.push(chunk.toString().toUpperCase());
    callback();
  }
});

const readStream = fs.createReadStream('LargeFile')
const writeStream = fs.createWriteStream('LargeFileDestination')

readStream.pipe(upperCaseTr).pipe(writeStream)

```

## Conclusion

Streams are a powerful feature of Nodejs that helps you to perform input/output operations efficiently. 
Node implements four types of Stream to cover all possible uses case and allow them to be combined through the pipe operator to enable you to implement robust solutions for your requirements.


