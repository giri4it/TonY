# Tony History Server

## Development

You can use the `gradlew` script in `TonY` root.
Make sure you allow execution permission (`chmod +x gradlew`) to script before running if encountering permission issue.
Assuming you are in the root `TonY` folder:

- To build the project:
```
$ ./gradlew :tony-history-server:build
```

- To run the app locally:

First, copy `tony-history-server/conf/application.example.conf` to `tony-history-server/conf/application.conf`
and set all the necessary configurations. Locally, you probably don't have Kerberos security set up, so you can
ignore setting the `keytab` properties.

```
$ ./gradlew :tony-history-server:runPlayBinary
```

**Note:** this will only reload when receiving new request, __not__ when file changes. For reloading on file changes (hot reloading):
```
$ ./gradlew :tony-history-server:runPlayBinary -t
```

- After the message `<=============> 100% EXECUTING ...` displays, go to <http://localhost:9000> on your browser to see the app.

Initially, there won't be any data, but there is some example data in `tony-history-server/example/tony-history`
you can load by copying it to `/tmp`:

```
cp -r tony-history-server/example/tony-history /tmp
```

Double-check that the `history` configs in `tony-history-server/conf/application.conf` point to `/tmp/tony-history`.

- To run tests:
```
$ ./gradlew :tony-history-server:testPlayBinary
```

For more info about developing with Play using Gradle, click [here](https://docs.gradle.org/current/userguide/play_plugin.html#play_continuous_build).


## Production

1. Create the production zip by running `./gradlew :tony-history-server:createPlayBinaryZipDist`.
The zip should be created in `tony-history-server/build/distributions`.
2. Copy the zip to your production host.
3. Unzip it.
4. Optional: Before running the production binary, all the configurations for Tony History Server should be
set in `$TONY_CONF_DIR/tony-site.xml`. You can look at the [sample](./conf/tony-site.sample.xml)
to set up your own `tony-site.xml`. If needed, run
```
# should contain `tony-site.xml` inside it
export TONY_CONF_DIR=/path/to/tony/config/folder`
```
5. `cd tony-history-server-*`
6. Run `bin/startTHS.sh`. See [script](./startTHS.sh) for more details.

Steps (1) and (2) can also be done together by running the `./buildAndDeploy.sh` script
(see [Deployment](#deployment) section below).

To stop the THS, run
```
bin/stopTHS.sh
```


### <a name='deployment'>Deployment</a>

Before using the script, ensure that you have set execution permission (`chmod +x buildAndDeploy.sh`)

- To bundle the history server app and copy it to another host
```
# must be run from the tony-history-server folder
$ ./buildAndDeploy.sh user hostname.test.abc.com
```

See [script](./buildAndDeploy.sh) for more details.
