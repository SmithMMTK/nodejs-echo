```bash


curl -v -H "Content-Type: application/json" -X POST -d '{"data":{"baseData":{"ver":2,"id":"SampleRunId","name":"MicrosoftSupportSampleWebtestResultUsingCurl","duration":"10.00:00:00","success":true,"runLocation":"RegionName","message":"SampleWebtestResult","properties":{"SampleProperty":"SampleValue"}},"baseType":"AvailabilityData"},"ver":1,"name":"Microsoft.ApplicationInsights.Metric","time":"2022-02-07T14:50:00+07:00","sampleRate":100,"iKey":"83563197-d77c-4477-b0c3-69d43418d69f","flags":0}' https://dc.applicationinsights.azure.com/v2/track --proxy http://10.137.8.44:2520




curl -v -H "Content-Type: application/json" -X POST -d '{"data":{"baseData":{"ver":2,"id":"SampleRunId","name":"MicrosoftSupportSampleWebtestResultUsingCurl","duration":"10.00:00:00","success":true,"runLocation":"RegionName","message":"SampleWebtestResult","properties":{"SampleProperty":"SampleValue"}},"baseType":"AvailabilityData"},"ver":1,"name":"Microsoft.ApplicationInsights.Metric","time":"2022-02-07T14:50:00+07:00","sampleRate":100,"iKey":"780d38a0-8b31-4113-ac60-b9c83d459831","flags":0}' https://dc.applicationinsights.azure.com/v2/track --proxy http://10.2.2.4:3128


```

```json
{"data":{"baseData":{"ver":2,"id":"SampleRunId","name":"MicrosoftSupportSampleWebtestResultUsingCurl","duration":"10.00:00:00","success":true,"runLocation":"RegionName","message":"SampleWebtestResult","properties":{"SampleProperty":"SampleValue"}},"baseType":"AvailabilityData"},"ver":1,"name":"Microsoft.ApplicationInsights.Metric","time":"2022-01-05T22:00:00.0000000Z","sampleRate":100,"iKey":"780d38a0-8b31-4113-ac60-b9c83d459831","flags":0}
```

```javascript

const express = require('express')
const app = express()
const port = 3000
var os = require('os')
const { timeStamp } = require('console')
var ip = require("ip")
const appInsights = require("applicationinsights");

appInsights.setup("83563197-d77c-4477-b0c3-69d43418d69f")
    .setAutoDependencyCorrelation(true)
    .setAutoCollectRequests(true)
    .setAutoCollectPerformance(true, true)
    .setAutoCollectExceptions(true)
    .setAutoCollectDependencies(true)
    .setAutoCollectConsole(true)
    .setUseDiskRetryCaching(true)
    .setSendLiveMetrics(true)
    .setDistributedTracingMode(appInsights.DistributedTracingModes.AI)
    .start();

// let client = appInsights.defaultClient;

// client.config.proxyHttpsUrl = "http://xxx.xxx.xxx.xxx";

var ifaces = os.networkInterfaces()
var pip = getIPAddress()

// Iterate over interfaces ...
var adresses = Object.keys(ifaces).reduce(function (result, dev) {
    return result.concat(ifaces[dev].reduce(function (result, details) {
      return result.concat(details.family === 'IPv4' && !details.internal ? [details.address] : []);
    }, []));
  });
  
  // Print the result
  console.log(adresses)


app.get('/', (req, res) =>
        {
            const ip = req.headers['x-forwarded-for'] || req.connection.remoteAddress;
            let ts = Date.now()
            res.send('Call from IP ' + req.connection.remoteAddress  + '  to ' + pip + ' timestamp: ' + ts + ' /n');
            console.log('Call from IP ' + req.connection.remoteAddress  + ' to ' + pip + ' timestamp: ' + ts + ' /n');
        });

app.get('/whocallme', (req, res) =>
  {
    const ip = req.headers['x-forwarded-for'] || req.connection.remoteAddress;
    let ts = Date.now()
    
    res.send('Call from IP ' + req.connection.remoteAddress  + '  to ' + pip + ' timestamp: ' + ts + ' /n');
    console.log('Call from IP ' + req.connection.remoteAddress  + ' to ' + pip + ' timestamp: ' + ts + ' /n');
});

function getIPAddress() {
  var interfaces = require('os').networkInterfaces();
  for (var devName in interfaces) {
    var iface = interfaces[devName];

    for (var i = 0; i < iface.length; i++) {
      var alias = iface[i];
      if (alias.family === 'IPv4' && alias.address !== '127.0.0.1' && !alias.internal)
        return alias.address;
    }
  }
  return '0.0.0.0';
}


app.listen(port, () => console.log(`Example app listening on port ${port}!`))



```