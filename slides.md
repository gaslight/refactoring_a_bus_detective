## Hi!
* I'm Chris
* I'm from Gaslight
* I do apprenticeship and other stuff

---

## Agenda
* A brief history of Bus Detective
* Learn you a GTFS
* The rewrite
* How Elixir helped
* How Web Components helped

---

## What's a Bus Detective?
* In February 2015, Gaslight moved downtown
* It was cold and people wanted to know when their bus would actually get there
* Kevin Rockwood discovered a GTFS feed for Metro

---

## What's a GTFS feed?
* General Transit Feed Specification
* Spec for Transit agencies to publish their data
* How Google Maps gets pubtran info

---

## GTFS Static
* Routes
* Stops
* Times
* Calendars
* Fare rules
* zipfile of CSV (http)

---

## GTFS Realtime
* Trip updates
* Vehicle positions
* Service alerts
* Protocol Buffers over http

---

## Let's build a Rails app
* And you always need a JS framework...
* EmberJS
* Cordova = native apps

---

## It grows into a Thing
* Increasing usage
* Metro adopts it
* Ads on busses

---

![Bus Ad](bus_ad.jpg)

---

## Monetization is hard :(
* Didn't wanna charge for app
* Other ideas didn't quite pan out
* 0 revenue is hard to sustain
* Led to some loss of interest

---

## Problems we had
* Performance
* Client side polling
* Hosting $
* Picking a JS framework

---

## The rewrite
* Tim Mecklem got tired of seeing the error page
* Plan was to just rewrite importer in Elixir
* It kinda got out of hand :)

---

## [Bus Detective NG](https://colum-busdetective.herokuapp.com)
* Elixir
* Web Components
* Moving busses
* PWA

---

## Why Elixir
* Language
* Framework
* Platform

---

## Elixir
* Functional language
* Immutability
* Ruby inspired syntax
* Easy transition

---

## My Favorite Elixir Features
* Pattern matching
* Pipeline operator

---

## Pattern matching
```elixir
  defp load_stop(conn, [stop_id], _options) do
    ...
  end

  defp load_stop(conn, [feed_id, stop_remote_id], options) do
    ...
  end

```

---

## Pipeline operator
```elixir
  defp find_exact_stop_time_update(
         %TripUpdate{stop_time_update: stop_time_updates},
         stop_sequence
       ) do
    stop_time_updates
    |> Enum.filter(&(is_nil(&1.stop_sequence) || &1.stop_sequence == stop_sequence))
    |> Enum.at(0)
    |> StopTimeUpdate.from_message()
  end
```

---

## Phoenix
* MVC Web framework for Elixir
* Inspired by Rails
* Ohio Made!
* Channels

---

## Elixir platform
* Erlang VM
* Designed for telco (Ericcson)
* 20+ yrs old

---

## OTP
* Open Telephony Platform
* High concurrency
* High availability
* Distributed

---

## OTP concepts
* Processes (GenServers)
* Supervisors
* Applications
* Let it Crash!

---

### GenServer responding to a message
```elixir
  def handle_call(
        {:find_stop_time, trip_remote_id, stop_sequence},
        _,
        %{realtime_data: realtime_data} = state
      ) do
    stop_time_update =
      StopTimeUpdateFinder.find_stop_time_update( realtime_data, trip_remote_id, stop_sequence)

    {:reply, {:ok, stop_time_update}, state}
  end
```

---

## Bus Detective NG
* OTP Umbrella App
* Bus Detective
* Bus Detective Web
* Importer
* Realtime

---

## Elixir wins
* So fast
* No polling
* Less $
* Letting it crash
* No architecture!

---

## What about the front end?

---

## EmberJS
* Very powerful
* Very opinionated
* Stay on top of upgrades
* We didn't :(

---

## Front-end rewrite
* Gave up on EmberJS :(
* Moved everything to SSR
* Sprinkled in some javascript
* Refactored to Web Components

---

## I like Web Components!
* Make your own HTML elements
* v1 now implemented in Chrome and Firefox
* and since IE is going to Chrome... :)

---

## bd-timestamp
```javascript
class Timestamp extends HTMLElement {
  ...
  connectedCallback () {
    const update = () => {
      this.innerHTML = this.displayedTimestamp;
      setTimeout(update, 1000);
    };
    update();
  }
}
customElements.define('bd-timestamp', Timestamp);
```

---

## It's just html...
```html
<bd-timestamp timestamp="2019-03-21T18:22:55+00:00"></bd-timestamp>
```

---

## Please allow me to climb onto my soapbox
* Remember jQuery and Coffeescript?
* Proprietary components will join this list

---

## Things that made Web Components fun
* Keep thy components dumb
* Data passed via attributes (or props)
* Re-render on data change
* Emit custom events

---

## `wc-state-reducers`
* emits new state on custom events
* passes data to components on state change
* kinda like a lighter, simpler redux
* 27 lines of js

---

## Moving busses explained 

---

### Listening for a message
```javascript
channel.on('vehicle_positions', message => {
  dispatch(document, 'updateVehiclePositions',  
    message.vehicle_positions);
  console.log('Received Vehicle Positions', message);
});
```

---

### Reducer
```javascript
const updateVehiclePositions = (state, vehiclePositions) => {
  return Object.assign({}, state, { vehiclePositions });
};
```

---

### Subscriber
```javascript
'bd-stop-map': 
  (
    { mapExpanded, vehiclePositions, tripShapes },
    element
  ) => {
    ...
    element.setAttribute('vehicle-positions',
    JSON.stringify(vehiclePositions));
  }
```

---

### Listening for attribute changes
```javascript
  static get observedAttributes () {
    return ['expanded', 'vehicle-positions', 'trip-shapes'];
  }

  attributeChangedCallback () {
    this.displayVehicles();
    ...
  }
```

---

### Rendering on the map
```javascript
this.vehiclePositions.forEach((vehiclePosition) => {
  ...
  let busMarker = Leaflet.marker([vehiclePosition.latitude, vehiclePosition.longitude], {icon: busIcon});
  busMarker.bindTooltip(
    `
    <div class="map-bus-label" 
      style="background-color: #${vehiclePosition.route_color}; color: #${vehiclePosition.route_text_color};">
      ${vehiclePosition.route_name}
    </div>
  `,
    {permanent: true, direction: 'top'});
  busMarker.addTo(this.map);
});
```

---

## It's open source!
[github.com/bus-detective/bus_detective_ng](https://github.com/bus-detective/bus_detective_ng)

---

## Also check out [open-wc.org](http://open-wc.org)
* project generators
* testing
* LitElement

---

## Stuff we didn't get to
* Cordova
* PWA
* Load your own city!


