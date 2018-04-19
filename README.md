# How to run

First of all, you need to configure Kibana to be able to use the email. Do that in [kibana/config/kibana.yml]. I used my Gmail account for testing, so either simply provide your username and password, or change your smtp to your likings.

Next get and build images to install the sentinl plugin

```
docker-compose build
```

After that run all the services

```
docker-compose up
```

You should be able to open Kibana at http://localhost:5601 after couple seconds. You'll need to wait for Logstash to import the [logstash/sample3.csv] file.

When you see available indices in Management section, create an index pattern with a name like `logstash*` to match them. After that, you should be able to browse the data in Discovery section (do not forget to change the time frame, as te default is last 15 minutes, but the data are from 2018-04-18).

Now it's time to create watcher. Head to *sentinl* section and hit *New* in the top right corner. Click on *Raw* and paste in content of [sentinl_watcher.json]. You'll need to change `to` (and potentially `from`) values in the **Email Action** to make sure you actually get the email - you can do it directly in *Raw* section, or in *Action* section.

Now all you need to do is click *Save* in the top right corner and the report should start flowing..

To be able to explore the payload you can use in reports a bit more, head back to *Management* section and create another index pattern - this time it'll look like `watcher*`. Now you will be able to switch to that pattern in *Discovery* section and see what is the content of `payload` which is what you can use in report body.

Reports use https://mustache.github.io/ for accessing the values in `payload`

You can also look at the transformation in the watcher - that was needed to be able to access the keys which have dots in name.