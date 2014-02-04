# Distributed Job Queue for Node.js backed by Redis

## Usage

```js
var JobQueue = require('redis-dist-job-queue');
var jobQueue = new JobQueue();

jobQueue.registerJob('thisIsMyJobTypeId', {
  perform: function(params, callback) {
    console.info(params.theString.toUpperCase());
    callback();
  },
});

jobQueue.registerJob('pleaseShutDownNow', {
  perform: function(params, callback) {
    console.info("starting shutdown");
    callback();

    jobQueue.shutdown(function() {
      console.info("shutdown complete");
    });
  },
});

jobQueue.start();


jobQueue.submitJob('thisIsMyJobTypeId', 'resource_id', {theString: "derp"}, function(err) {
  if (err) throw err;
  console.info("job submitted");
});

jobQueue.submitJob('pleaseShutDownNow', 'resource_id_2', null, function(err) {
  if (err) throw err;
  console.info("cleanup job submitted");
});

```
