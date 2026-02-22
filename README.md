# flightaware-flightradar24
Unraid template for https://github.com/Thom-x/docker-fr24feed-piaware-dump1090

## FlightAware

Register on https://flightaware.com/account/join/.

Run :

```
docker run -it --rm \
	-e 'SERVICE_ENABLE_DUMP1090=false' \
	-e 'SERVICE_ENABLE_HTTP=false' \
	-e 'SERVICE_ENABLE_FR24FEED=false' \
	-e 'SERVICE_ENABLE_PIAWARE=false' \
	thomx/fr24feed-piaware /usr/bin/piaware -plainlog
```

When the container starts you should see the feeder id, note it. Wait 5 minutes and you should see a new receiver at https://fr.flightaware.com/adsb/piaware/claim (use the same IP as your docker host), claim it and exit the container.
If not, just open https://fr.flightaware.com/adsb/piaware/claim/ `your-feeder-id`

Add the environment variable `PIAWARE_FEEDER_DASH_ID` with your feeder id.

| Environment Variable         | Configuration property | Default value |
| ---------------------------- | ---------------------- | ------------- |
| `PIAWARE_FEEDER_DASH_ID`     | `feeder-id (required)` | empty         |
| `PIAWARE_RECEIVER_DASH_TYPE` | `receiver-type`        | `other`       |
| `PIAWARE_RECEIVER_DASH_HOST` | `receiver-host`        | `127.0.0.1`   |
| `PIAWARE_RECEIVER_DASH_PORT` | `receiver-port`        | `30005`       |

Ex : `-e 'PIAWARE_RECEIVER_DASH_TYPE=other'`

## FlightRadar24

Run :

```
docker run -it --rm \
	-e 'SERVICE_ENABLE_DUMP1090=false' \
	-e 'SERVICE_ENABLE_HTTP=false' \
	-e 'SERVICE_ENABLE_PIAWARE=false' \
	-e 'SERVICE_ENABLE_FR24FEED=false' \
	thomx/fr24feed-piaware /bin/bash
```

Then : `/fr24feed/fr24feed/fr24feed --signup` and follow the instructions. For technical steps, your answer doesn't matter. We just need the sharing key at the end.

Finally, to see the sharing, key run `cat /etc/fr24feed.ini`. You can now exit the container.

Add the environment variable `FR24FEED_FR24KEY` with your sharing key.

| Environment Variable                  | Configuration property                                                                                              | Default value     |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------- | ----------------- |
| `FR24FEED_RECEIVER`                   | `receiver`                                                                                                          | `beast-tcp`       |
| `FR24FEED_FR24KEY`                    | `fr24key (required)`                                                                                                | empty             |
| `FR24FEED_HOST`                       | `host`                                                                                                              | `127.0.0.1:30005` |
| `FR24FEED_BS`                         | `bs`                                                                                                                | `no`              |
| `FR24FEED_RAW`                        | `raw`                                                                                                               | `no`              |
| `FR24FEED_LOGMODE`                    | `logmode`                                                                                                           | `1`               |
| `FR24FEED_LOGPATH`                    | `logpath`                                                                                                           | `/tmp`            |
| `FR24FEED_MLAT`                       | `mlat`                                                                                                              | `no`              |
| `FR24FEED_MLAT_DASH_WITHOUT_DASH_GPS` | `mlat-without-gps`                                                                                                  | `no`              |
| `SYSTEM_FR24FEED_ULIMIT_N`            | Enforce ulimit like docker <=22 to prevent CPU issues (-1 means not enforced), recommended value when crash 1048576 | -1                |

Ex : `-e 'FR24FEED_FR24KEY=0123456789'`
