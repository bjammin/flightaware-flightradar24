# flightaware-flightradar24
Unraid template for https://github.com/Thom-x/docker-fr24feed-piaware-dump1090

There was a previous community applications template, however this did not seem to work anymore.

Simply go through these steps to setup your feed with FlightAware and FlightRadar24.  If you already have these keys, you can skip these steps.

Then "Create and run container"

## FlightAware

Register on https://flightaware.com/account/join/.

At the unRaid console, run :

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

Note down the feeder ID, we will use it in the template variable `PIAWARE_FEEDER_DASH_ID`.


## FlightRadar24

At the unRaid console, run :

```
docker run -it --rm \
	-e 'SERVICE_ENABLE_DUMP1090=false' \
	-e 'SERVICE_ENABLE_HTTP=false' \
	-e 'SERVICE_ENABLE_PIAWARE=false' \
	-e 'SERVICE_ENABLE_FR24FEED=false' \
	thomx/fr24feed-piaware /bin/bash
```

Then : `/fr24feed/fr24feed/fr24feed --signup` and follow the instructions. For technical steps, your answer doesn't matter. We just need the sharing key at the end.

Note down the feed key, we will use it in the template variable `FR24FEED_FR24KEY`.

## Create and run container

Start the container using PIAWARE_FEEDER_DASH_ID and FR24FEED_FR24KEY obtained above.

Also populate HTML_SITE_LAT with your latitude, HTML_SITE_LON with your longitude, and HTML_SITE_ALT with your altitude (in feet).

Launch the container, and browse to yourIP:8080 and you should see the map!
