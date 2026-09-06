

- Looking into https://nickballou.com/blog/custom-wowchemy/ 

- The idea is to replicate the membership page of the Open Traits Network (https://opentraits.org/members/) on the EMI website.

Looking for old li_compact.html files 
- https://github.com/forrtproject/forrtproject.github.io/tree/eb55900da7da50390ca23a929794fde9073c7df5


This whole story turns out to be a bit more complicated than I thought. I will need to spend more time on this.

First it seems that the wowchemy is now hugo blox.
Second Hugo blox is not compatible with the latest version of Hugo.

Switching hugo version is not has straightforward.
Installing a specific version neither.

https://www.gigigatgat.ca/en/posts/install-specific-hugo-version/

https://github.com/gohugoio/hugo/releases/tag/v0.121.1



/Users/pma/.gem/ruby/3.3.5/bin:/Users/pma/.rubies/ruby-3.3.5/lib/ruby/gems/3.3.0/bin:/Users/pma/.rubies/ruby-3.3.5/bin:/Users/pma/.pyenv/shims:/Users/pma/.nvm/versions/node/v17.8.0/bin:/Users/pma/.yarn/bin:/Users/pma/.config/yarn/global/node_modules/.bin:/Users/pma/.rbenv/shims:/Users/pma/.cargo/bin:/Users/pma/.local/bin:/usr/local/bin:/bin:/sbin:/usr/local/sbin:/usr/local/bin:/usr/local/texlive/2023/bin/universal-darwin:/Users/pma/bin:/usr/local/bin:/Users/pma/mambaforge/lib/python3.9/site-packages:/usr/local/texlive/2023/bin/universal-darwin:/Users/pma/.nvm/versions/node/v17.8.0/bin:/Users/pma/.yarn/bin:/Users/pma/.config/yarn/global/node_modules/.bin:/Users/pma/.rbenv/shims:/Users/pma/.cargo/bin:/Users/pma/.local/bin:/Users/pma/mambaforge/bin:/Users/pma/mambaforge/condabin:/usr/local/bin:/bin:/sbin:/usr/local/sbin:/usr/local/bin:/usr/local/texlive/2023/bin/universal-darwin:/Users/pma/bin:/usr/local/bin:/Users/pma/mambaforge/lib/python3.9/site-packages:/usr/local/texlive/2023/bin/universal-darwin:/opt/local/bin:/opt/local/sbin:/usr/local/bin:/System/Cryptexes/App/usr/bin:/usr/bin:/bin:/usr/sbin:/sbin:/var/run/com.apple.security.cryptexd/codex.system/bootstrap/usr/local/bin:/var/run/com.apple.security.cryptexd/codex.system/bootstrap/usr/bin:/var/run/com.apple.security.cryptexd/codex.system/bootstrap/usr/appleinternal/bin:/opt/X11/bin:/usr/local/share/dotnet:~/.dotnet/tools:/Library/Frameworks/Mono.framework/Versions/Current/Commands:/Applications/quarto/bin:/Applications/iTerm.app/Contents/Resources/utilities


How to embedd a map 

https://blog.gpkb.org/posts/hugo-static-site-maps/

7.114334,46.777595,7.197247,46.822506

pmtiles extract \
        https://build.protomaps.com/20241014.pmtiles \
        mymap.pmtiles \
        --bbox=7.114334,46.777595,7.197247,46.822506


https://osgav.run/

Hinode theme has a leaflet module https://gethinode.com/docs/getting-started/introduction/




My leaflet-map.js

const attribution = '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
const tile = 'https://tile.openstreetmap.org/{z}/{x}/{y}.png'

document.querySelectorAll('div.leaflet-map').forEach(map => {
  const viewLat = map.getAttribute('data-leaflet-view-lat')
  const viewLong = map.getAttribute('data-leaflet-view-long')
  const zoom = map.getAttribute('data-leaflet-view-zoom')
  const popup = map.getAttribute('data-leaflet-popup-caption')
  const popupLat = map.getAttribute('data-leaflet-popup-lat')
  const popupLong = map.getAttribute('data-leaflet-popup-long')
  const view = [viewLat, viewLong]

  if (viewLat === null || viewLong === null || zoom === null) {
    console.error('leaflet-map.js: expected lat, long, and zoom')
  } else {
    const bind = L.map(map).setView(view, zoom)
    L.tileLayer(tile, { attribution }).addTo(bind)

    if (popup !== null && popupLat !== null && popupLong !== null) {
      L.marker([popupLat, popupLong]).addTo(bind)
        .bindPopup(popup)
        .openPopup()
    }
  }
})


My layout/partial/assets/map.html

<!-- 
    Copyright © 2024 The Hinode Team / Mark Dumay. All rights reserved.
    Use of this source code is governed by The MIT License (MIT) that can be found in the LICENSE file.
    Visit gethinode.com/license for more details.
-->

{{ $error := false }}

<!-- Validate arguments -->
{{ if partial "utilities/IsInvalidArgs.html" (dict "structure" "map" "args" .Params) }}
    {{ errorf "Invalid arguments: %s" .Position -}}
    {{ $error = true }}
{{ end }}

<!-- Initialize arguments -->

{{- $id := printf "leaflet-map-%d" .Ordinal -}}
{{ with index . "id" }}
    {{ $id = . }}
{{ end }}

{{ $class := index . "class" | default "ratio ratio-16x9 w-100 mx-auto" }}

{{ $error := false }}
{{ $viewLat := 52.377 }}
{{ $viewLong := 4.90 }}
{{ $zoom := 13 }}
{{ $popup := "" }}
{{ $popupLat := ""  }}
{{ $popupLong := "" }}

{{- with index . "lat" }}{{ $viewLat = . }}{{ end -}}
{{- with index . "long" }}{{ $viewLong = . }}{{ end -}}
{{- with index . "zoom" }}{{ $zoom = . }}{{ end -}}
{{- with index . "popup" }}{{ $popup = . }}{{ end -}}
{{- with index . "popup-lat" }}{{ $popupLat = . }}{{ end -}}
{{- with index . "popup-long" }}{{ $popupLong = . }}{{ end -}}

<!-- Main code -->
{{- if not $error -}}
    <div id="{{ $id }}" class="leaflet-map {{ $class }}"
        {{ with $viewLat }}data-leaflet-view-lat="{{ . }}"{{ end }}
        {{ with $viewLong }}data-leaflet-view-long="{{ . }}"{{ end }}
        {{ with $zoom }}data-leaflet-view-zoom="{{ . }}"{{ end }}
        {{ with $popup }}data-leaflet-popup-caption="{{ . }}"{{ end }}
        {{ with $popupLat }}data-leaflet-popup-lat="{{ . }}"{{ end }}
        {{ with $popupLong }}data-leaflet-popup-long="{{ . }}"{{ end }}>
    </div>
{{- end -}}

######

Frontmatter in a content file

```yaml

---
title: Pierre-Marie Allard
firstname: Pierre-Marie
lastname: Allard
institution: COMMONS Lab, Department of Biology, University of Fribourg, Switzerland
email: pierre-marie.allard@unifr.ch
orcid: https://orcid.org/0000-0003-3389-2191
wikidata: https://www.wikidata.org/wiki/Q56451301
scholia: https://scholia.toolforge.org/author/Q56451301
thumbnail:
  url: /img/sunrise.jpg
  author: Harris Vo
  authorURL: https://unsplash.com/@hoanvokim
  origin: https://unsplash.com/photos/ZX6BPboJrYk
  originName: Unsplash
modules: ["leaflet"]
latitude: 52.377
longitude: 4.900
popup: "Amsterdam Central Station is it here?"
type: members
---

```

Such file in layouts/members/single/body.html

```html
<div class="row">
    <!-- Left column: Member Information -->
    <div class="col-md-8">
      {{- /* Display Member Information */ -}}
      {{ with .Params.author }}
        <p class="author-entry display-4 mb-3">
          <strong>{{ . }}</strong>
        </p>
      {{ end }}
  
      {{- /* Display Institution, Email, ORCID, Wikidata, and Scholia */ -}}
      {{ with .Params }}
        <ul class="metadata-list list-unstyled">
          {{ with .institution }}
            <li><strong>Institution or place of work:</strong> {{ . }}</li>
          {{ end }}
          {{ with .email }}
            <li><strong>Email:</strong> <a href="mailto:{{ . }}">{{ . }}</a></li>
          {{ end }}
          {{ with .orcid }}
            <li><strong>ORCID:</strong> <a href="{{ . }}">{{ . }}</a></li>
          {{ end }}
          {{ with .wikidata }}
            <li><strong>Wikidata:</strong> <a href="{{ . }}">{{ . }}</a></li>
          {{ end }}
          {{ with .scholia }}
            <li><strong>Scholia:</strong> <a href="{{ . }}">{{ . }}</a></li>
          {{ end }}
        </ul>
      {{ end }}
    </div>
  
    <!-- Right column: Thumbnail Image -->
    <div class="col-md-4">
      {{- /* Extract thumbnail information */ -}}
      {{ with .Params.thumbnail }}
        <div class="post-thumbnail mb-5">
          <img src="{{ .url }}" alt="Thumbnail" class="img-fluid">
          <div class="thumbnail-attribution">
            Photo by <a href="{{ .authorURL }}">{{ .author }}</a> on 
            <a href="{{ .origin }}">{{ .originName }}</a>
          </div>
        </div>
      {{ end }}
    </div>
  </div>
  
  <!-- Map Section -->
  {{- /* Render the Map using the Leaflet map partial */ -}}
  {{ with .Params }}
    {{ if and .latitude .longitude .popup }}
      <div class="map-container mb-5">
        {{ partial "assets/map.html" (dict "lat" .latitude "long" .longitude "popup" .popup "popup-lat" .latitude "popup-long" .longitude) }}
      </div>
    {{ end }}
  {{ end }}

  
  
  {{- /* Render the rest of the content */ -}}
  {{ partial "utilities/ProcessContent" (dict "page" .Page "raw" .RawContent) }}
```

and layouts/partials/assets/map.html

```html
<!-- 
    Copyright © 2024 The Hinode Team / Mark Dumay. All rights reserved.
    Use of this source code is governed by The MIT License (MIT) that can be found in the LICENSE file.
    Visit gethinode.com/license for more details.
-->

{{ $error := false }}

<!-- Validate arguments -->
{{ if partial "utilities/IsInvalidArgs.html" (dict "structure" "map" "args" .Params) }}
    {{ errorf "Invalid arguments: %s" .Position -}}
    {{ $error = true }}
{{ end }}

<!-- Initialize arguments -->

{{- $id := printf "leaflet-map-%d" .Ordinal -}}
{{ with index . "id" }}
    {{ $id = . }}
{{ end }}

{{ $class := index . "class" | default "ratio ratio-16x9 w-100 mx-auto" }}

{{ $error := false }}
{{ $viewLat := 52.377 }}
{{ $viewLong := 4.90 }}
{{ $zoom := 13 }}
{{ $popup := "" }}
{{ $popupLat := ""  }}
{{ $popupLong := "" }}

{{- with index . "lat" }}{{ $viewLat = . }}{{ end -}}
{{- with index . "long" }}{{ $viewLong = . }}{{ end -}}
{{- with index . "zoom" }}{{ $zoom = . }}{{ end -}}
{{- with index . "popup" }}{{ $popup = . }}{{ end -}}
{{- with index . "popup-lat" }}{{ $popupLat = . }}{{ end -}}
{{- with index . "popup-long" }}{{ $popupLong = . }}{{ end -}}

<!-- Main code -->
{{- if not $error -}}
    <div id="{{ $id }}" class="leaflet-map {{ $class }}"
        {{ with $viewLat }}data-leaflet-view-lat="{{ . }}"{{ end }}
        {{ with $viewLong }}data-leaflet-view-long="{{ . }}"{{ end }}
        {{ with $zoom }}data-leaflet-view-zoom="{{ . }}"{{ end }}
        {{ with $popup }}data-leaflet-popup-caption="{{ . }}"{{ end }}
        {{ with $popupLat }}data-leaflet-popup-lat="{{ . }}"{{ end }}
        {{ with $popupLong }}data-leaflet-popup-long="{{ . }}"{{ end }}>
    </div>
{{- end -}}
```



My leaflet-map.js 

```javascript
const attribution = '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
const tile = 'https://tile.openstreetmap.org/{z}/{x}/{y}.png'

document.querySelectorAll('div.leaflet-map').forEach(map => {
  const viewLat = map.getAttribute('data-leaflet-view-lat')
  const viewLong = map.getAttribute('data-leaflet-view-long')
  const zoom = map.getAttribute('data-leaflet-view-zoom')
  const popup = map.getAttribute('data-leaflet-popup-caption')
  const popupLat = map.getAttribute('data-leaflet-popup-lat')
  const popupLong = map.getAttribute('data-leaflet-popup-long')
  const view = [viewLat, viewLong]

  if (viewLat === null || viewLong === null || zoom === null) {
    console.error('leaflet-map.js: expected lat, long, and zoom')
  } else {
    const bind = L.map(map).setView(view, zoom)
    L.tileLayer(tile, { attribution }).addTo(bind)

    if (popup !== null && popupLat !== null && popupLong !== null) {
      L.marker([popupLat, popupLong]).addTo(bind)
        .bindPopup(popup)
        .openPopup()
    }
  }
})
```
With such set of scripts and partials, I can render a map on the member page.
This map displays a single marker.

I would like to be able to render multiple markers and their popup on the map.
The expansion of the script should be able to handle single or multiple markers and popups.

I thought that the frontmatter would look like this ?

```yaml
---
title: Pierre-Marie Allard
firstname: Pierre-Marie
lastname: Allard
institution: COMMONS Lab, Department of Biology, University of Fribourg, Switzerland
email: pierre-marie.allard@unifr.ch
orcid: https://orcid.org/0000-0003-3389-2191
wikidata: https://www.wikidata.org/wiki/Q56451301
scholia: https://scholia.toolforge.org/author/Q56451301
thumbnail:
  url: /img/sunrise.jpg
  author: Harris Vo
  authorURL: https://unsplash.com/@hoanvokim
  origin: https://unsplash.com/photos/ZX6BPboJrYk
  originName: Unsplash
modules: ["leaflet"]
latitude: [52.377, 52.378]
longitude: [4.900, 4.901]
popup: ["Amsterdam Central Station is it here?", "Amsterdam Central Station is it here?"]
type: members
---
```

Could you help me adapt the previous script where it is needed ?



Now I also have layouts/partials/assets/custom-map.html

```html
{{ $error := false }}

{{/* Validate arguments */}}
{{ if partial "utilities/IsInvalidArgs.html" (dict "structure" "custom-map" "args" . "group" "partial") }}
  {{- errorf "partial [assets/list.html] - Invalid arguments" -}}
  {{ $error = true }}
{{ end }}

{{/* Initialize arguments */}}
{{- $list := .list -}}
{{- $class := .class -}}

{{/* Main code */}}
{{ if not $error }}
  {{- range $index, $item := $list }}
    {{ with $item.Params }}
      {{ if and $item.Params.latitude $item.Params.longitude $item.Params.popup }}
        <div class="container-fluid p-0">
          <div class="map-container mb-5">
            {{ partial "assets/map.html" (dict "lat" $item.Params.latitude "long" $item.Params.longitude "popup" $item.Params.popup "popup-lat" $item.Params.latitude "popup-long" $item.Params.longitude) }}
          </div>
        </div>

        {{- /* Stops after the first valid item */ -}}
        {{- break -}}

      {{ end }}
    {{ end }}
  {{ end }}
{{ end }}
```

Which is able to iterate over a list of items.
For now it is just a draft and it breaks after one iteration.
Could you help me adapt the previous script where it is needed so that it iterates over the list of items and displays all the markers and popups on one single map ?




In this script.

const attribution = '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
const tile = 'https://tile.openstreetmap.org/{z}/{x}/{y}.png'

document.querySelectorAll('div.leaflet-map').forEach(map => {
  const viewLat = map.getAttribute('data-leaflet-view-lat')
  const viewLong = map.getAttribute('data-leaflet-view-long')
  const zoom = map.getAttribute('data-leaflet-view-zoom')
  const popupData = map.getAttribute('data-leaflet-popup-caption')  // This will now store a JSON string

  if (viewLat === null || viewLong === null || zoom === null) {
    console.error('leaflet-map.js: expected lat, long, and zoom')
  } else {
    const bind = L.map(map).setView([viewLat, viewLong], zoom)
    L.tileLayer(tile, { attribution }).addTo(bind)

    // Parse popupData as an array
    const popups = popupData ? JSON.parse(popupData) : []
    const popupLatitudes = map.getAttribute('data-leaflet-popup-lat') ? JSON.parse(map.getAttribute('data-leaflet-popup-lat')) : []
    const popupLongitudes = map.getAttribute('data-leaflet-popup-long') ? JSON.parse(map.getAttribute('data-leaflet-popup-long')) : []

    if (popups.length && popupLatitudes.length && popupLongitudes.length) {
      for (let i = 0; i < popups.length; i++) {
        if (popupLatitudes[i] && popupLongitudes[i]) {
          L.marker([popupLatitudes[i], popupLongitudes[i]]).addTo(bind)
            .bindPopup(popups[i])
            .openPopup()
        }
      }
    }
  }
})


Is it possible to make sure that this will work for entries with a single marker and popup as well as for entries with multiple markers and popups ?

That is for frontmatters such 

title: Pierre-Marie Allard
firstname: Pierre-Marie
lastname: Allard
institution: COMMONS Lab, Department of Biology, University of Fribourg, Switzerland
email: pierre-marie.allard@unifr.ch
orcid: https://orcid.org/0000-0003-3389-2191
wikidata: https://www.wikidata.org/wiki/Q56451301
scholia: https://scholia.toolforge.org/author/Q56451301
thumbnail:
  url: /img/sunrise.jpg
  author: Harris Vo
  authorURL: https://unsplash.com/@hoanvokim
  origin: https://unsplash.com/photos/ZX6BPboJrYk
  originName: Unsplash
modules: ["leaflet"]
latitude: [52.377, 52.378]
longitude: [4.900, 4.901]
popup: ["Amsterdam Central Station is it here?", "Amsterdam Central Station is it here?"]
type: members
---

or

title: Pierre-Marie Allard
firstname: Pierre-Marie
lastname: Allard
institution: COMMONS Lab, Department of Biology, University of Fribourg, Switzerland
email: pierre-marie.allard@unifr.ch
orcid: https://orcid.org/0000-0003-3389-2191
wikidata: https://www.wikidata.org/wiki/Q56451301
scholia: https://scholia.toolforge.org/author/Q56451301
thumbnail:
  url: /img/sunrise.jpg
  author: Harris Vo
  authorURL: https://unsplash.com/@hoanvokim
  origin: https://unsplash.com/photos/ZX6BPboJrYk
  originName: Unsplash
modules: ["leaflet"]
latitude: 52.377
longitude: 4.900
popup: "Amsterdam Central Station is it here?"
type: members
---


OK this doesnt work. I will need to find a way to make it work.

Lets rather work on the map.html

Currently the map.html is 

```html
<!-- 
    Copyright © 2024 The Hinode Team / Mark Dumay. All rights reserved.
    Use of this source code is governed by The MIT License (MIT) that can be found in the LICENSE file.
    Visit gethinode.com/license for more details.
-->

{{ $error := false }}

<!-- Validate arguments -->
{{ if partial "utilities/IsInvalidArgs.html" (dict "structure" "map" "args" .Params) }}
    {{ errorf "Invalid arguments: %s" .Position -}}
    {{ $error = true }}
{{ end }}

<!-- Initialize arguments -->

{{- $id := printf "leaflet-map-%d" .Ordinal -}}
{{ with index . "id" }}
    {{ $id = . }}
{{ end }}

{{ $class := index . "class" | default "ratio ratio-16x9 w-100 mx-auto" }}

{{ $error := false }}
{{ $viewLat := 52.377 }}
{{ $viewLong := 4.90 }}
{{ $zoom := 13 }}
{{ $popup := "" }}
{{ $popupLat := ""  }}
{{ $popupLong := "" }}

{{- with index . "lat" }}{{ $viewLat = . }}{{ end -}}
{{- with index . "long" }}{{ $viewLong = . }}{{ end -}}
{{- with index . "zoom" }}{{ $zoom = . }}{{ end -}}
{{- with index . "popup" }}{{ $popup = . }}{{ end -}}
{{- with index . "popup-lat" }}{{ $popupLat = . }}{{ end -}}
{{- with index . "popup-long" }}{{ $popupLong = . }}{{ end -}}

{{- if not $error -}}
  <div id="{{ $id }}" class="leaflet-map {{ $class }}"
      {{ with $viewLat }}data-leaflet-view-lat="{{ index $viewLat 0 }}"{{ end }}
      {{ with $viewLong }}data-leaflet-view-long="{{ index $viewLong 0 }}"{{ end }}
      {{ with $zoom }}data-leaflet-view-zoom="{{ . }}"{{ end }}
      {{ with $popup }}
        data-leaflet-popup-caption='{{ jsonify $popup }}'
        data-leaflet-popup-lat='{{ jsonify $popupLat }}'
        data-leaflet-popup-long='{{ jsonify $popupLong }}'
      {{ end }}>
  </div>
{{- end -}}
```

Let's try to make it work for multiple markers and popups.

It should handle the cases where there is only one marker and popup as well as the cases where there are multiple markers and popups.

That is when value of latitude and longitude are arrays and when the value of popup is an array, or when the value of latitude and longitude are single values and the value of popup is a single value.

Eg.

```yaml
---
title: Pierre-Marie Allard
firstname: Pierre-Marie
lastname: allardpm
latitude: [52.377, 52.378]
longitude: [4.900, 4.901]
popup: ["Amsterdam Central Station is it here?", "Amsterdam Central Station is it here?"]
---
```

or

```yaml
---
title: Pierre-Marie Allard
firstname: Pierre-Marie
lastname: allardpm
latitude: 52.377
longitude: 4.900
popup: "Amsterdam Central Station is it here?"
---
```


It's working ! Almost good :)
I can now display the multiple markers.

One thing is not OK is that I can view all popups. I can only view the last one.

Does this has to do with 


const attribution = '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
const tile = 'https://tile.openstreetmap.org/{z}/{x}/{y}.png'

document.querySelectorAll('div.leaflet-map').forEach(map => {
  const viewLat = map.getAttribute('data-leaflet-view-lat')
  const viewLong = map.getAttribute('data-leaflet-view-long')
  const zoom = map.getAttribute('data-leaflet-view-zoom')
  const popupData = map.getAttribute('data-leaflet-popup-caption')  // This will now store a JSON string

  if (viewLat === null || viewLong === null || zoom === null) {
    console.error('leaflet-map.js: expected lat, long, and zoom')
  } else {
    const bind = L.map(map).setView([viewLat, viewLong], zoom)
    L.tileLayer(tile, { attribution }).addTo(bind)

    // Parse popupData as an array
    const popups = popupData ? JSON.parse(popupData) : []
    const popupLatitudes = map.getAttribute('data-leaflet-popup-lat') ? JSON.parse(map.getAttribute('data-leaflet-popup-lat')) : []
    const popupLongitudes = map.getAttribute('data-leaflet-popup-long') ? JSON.parse(map.getAttribute('data-leaflet-popup-long')) : []

    if (popups.length && popupLatitudes.length && popupLongitudes.length) {
      for (let i = 0; i < popups.length; i++) {
        if (popupLatitudes[i] && popupLongitudes[i]) {
          L.marker([popupLatitudes[i], popupLongitudes[i]]).addTo(bind)
            .bindPopup(popups[i])
            .openPopup()
        }
      }
    }
  }
})


