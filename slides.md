## Hi!
* I'm Chris
* I'm from Gaslight
* I do apprenticeship and other stuff

---

## Agenda
* A brief history of Bus Detective
* Learn you a GTFS
* The rewrite
* What we learned

---

## What's a Bus Detective?
* In February 2015, Gaslight moved downtown
* It was cold and people wanted to know when their bus would actually get there
* Kevin Rockwood discovered a GTFS feed for Metro
* He wrote some code...

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

## Monetization is hard :(
* Ideas didn't quite pan out
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
* Tim got tired of seeing the error page
* Plan was to just rewrite importer in Elixir
* It kinda got out of hand :)

---

## [Bus Detective NG](app.busdetective.com)

---

## Why Elixir?
* Functional language
* Erlang VM
* Ruby inspired syntax

---

## Phoenix
* MVC Web framework for Elixir
* Inspired by Rails
* Ohio Made!
* Ridonkulus performance
* Channels

---

## My Favorite Elixir Features
* Pattern matching
* Pipeline operator

---

## Searching for Stops
* StopController
* 

---

## Erlang
* Designed for telco (Ericcson)
* 20+ yrs old
* Prolog inspired syntax

---

## OTP
* 20 yrs of
  * Distributed
  * Scalable
  * Fault tolerant

---

## OTP ideas
* Processes
* Supervisors
* Applications
* Let it Crash!

---

## Bus Detective NG
* Umbrella App
* Bus Detective
* Bus Detective Web
* Importer
* Realtime

---

## GenServer
* A stateful process
* Responds to messages
* Managed by supervisors

---

# Let's see Realtime
* Application
* VehiclePositions

---

## Elixir wins
* So fast
* No polling
* upsert

---

## What about the front end?

---

## EmberJS
* Very powerful
* Very opinionated
* Stay on top of upgrades
* We didn't :(

---

## Do we need a SPA?
* Gaslight "Rule 0"
* But mebbe not...
* Let's try not!

---

## But we eventually do need some javascript
How to do?

---

## I like Web Components!
* Make your own HTML elements
* v1 now implemented in Chrome and Firefox
* and since IE is going to Chrome... :)

---

## Please allow me to climb onto my soapbox
* Remember jQuery and Coffeescript?
* Proprietary components will join this list

---

### [Refactoring to Web Components](https://github.com/bus-detective/bus_detective_ng/commit/87bdedf25d3b30f2243c4475217ef799d12c2394?diff=split)

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

---

## Let's see some!
Let's look at bd-map all growned up

---

## Also check out [open-wc.org](http://open-wc.org)
* project generators
* testing
* LitElement

---

## Bonus things!
* Moving busses on the map
* PWA
* Load your own city!


