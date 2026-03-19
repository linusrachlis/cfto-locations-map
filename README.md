# Community Fridges Toronto Interactive Locations Map

## Overview

The communityfridgesto.org interactive locations map is implemented as a single
HTML block embedded into the "home" page on the Squarespace site using a "Code"
block with mode "HTML" and "Display Source Code" disabled. The dimensions of
the block are set in the Squarespace editor, and then everything that appears
inside is controlled by the HTML.

## How to update the map

### General

Make your updates in `src/index.html` and open the file in a browser to test
them out. When ready, simply copy everything between (but not including)
`<body>` and `</body>`, and paste it into the "Code" block in the Squarespace
editor, then click Save at the top.

### Updating locations

Look for the line `const sites = ...`. Each block that follows with `{ ... }`
is the information for one location. The fields are:

* `lat`: latitude (for Toronto, this is always a positive decimal number)
* `lng`: longitude (for Toronto, this is always a negative decimal number)
* `addr`: the human-readable address/location name. This has no effect on where
the pin is placed.
* `desc` *(optional)*: a few words on how to actually locate the fridge when
you go to the address (keep this short since it displays inside the
tooltip when you click on a pin).

You can add as many locations as you want. Always make sure the closing `}` is
followed by a comma.

#### Getting latitude and longitude

1. Use a desktop/laptop computer (not a phone).
2. Open Google Maps (https://google.com/maps) or OpenStreetMap
   (https://www.openstreetmap.org/).
3. Find the location and zoom all the way in.
4. Right-click or control-click on the exact spot you want.
    * Google Maps: the coordinates will be the first option in the menu that
    appears. Click to copy.
    * OpenStreetMap: click "Show Address" from the menu that appears, then copy
    the coordinates from the sidebar.
5. After pasting into the code, reformat the coordinates into the `lat` and
   `lng` fields (the negative one is always `lng` for Toronto locations).

### Updating other code

#### CSS

The `<style>...</style>` part of `index.html` is just a test harness for
development and not part of what you copy into the Squarespace block. The CSS
that actually goes in Squarespace is defined and injected into the header via
Javascript, which avoids the need to deploy code changes to more than one place
in Squarespace. To find it, look for lines with
`document.createElement('style')`.

#### Map settings

Look for the `L.map(...)` statement to see Leaflet settings. They are all
commented.

## Tile Providers

Leaflet is a code library for running an interactive map on a website, but it
does not come with any built-in map images. Map images are called "tiles"
because for any given square area of the world, there are many different
available map renderings, with various levels of quality, detail, styles, and
purposes. These images have to come from somewhere, and that requires web
hosting (i.e. compute time and internet bandwidth). That's what a "tile
provider" is -- some web service that provides the images displayed by Leaflet.

The images on this map will look a little different during testing/development
than they look on the actual website, due to some logic that changes the tile
provider based on what's in the browser's address bar. For testing we just
piggyback on the OpenStreetMap tile images, which is allowed for limited
purposes but not allowed for use on a real website. For the real website we
switch to the [MapTiler](https://www.maptiler.com/) tile provider service.

MapTiler is allowed for real websites and has a free tier for non-commercial
purposes, which is limited to 5,000 map views per month and 100,000 tiles
served per month. A typical visitor would use one map view and 10-50 tiles
served depending how much they zoom and pan around. We have a free MapTiler
account; ask on Slack if you need the login. Alternatively, you can simply sign
up for a new free account, generate a new API key, and replace the one in
`src/index.html` (it's the `apiKey` parameter under the
`L.tileLayer('https://api.maptiler.com/...)`
section).
