# 🚆 RailPulse

### Real-time passenger flow intelligence for Mumbai local trains.

RailPulse is a crowd-intelligence concept designed as an extension to **M-Indicator**.

The idea is to use participating passengers' journeys to build a **real-time understanding of crowd density and passenger movement across Mumbai's local railway network**.

Instead of simply showing that a train is arriving, RailPulse aims to answer:

> **How crowded is the train right now, and what is likely to happen to that crowd at the upcoming stations?**

---

## 💡 The Core Idea

When a passenger selects a destination and train, their journey can be associated with the train they actually board.

As the train moves between stations, the system observes participating passengers:

```text
Station
   ↓
Passengers board
   ↓
Train moves
   ↓
Passengers get off
   ↓
Crowd estimate updates
   ↓
Next station
```

For example:

```text
Dombivli
   ↓
Train starts with X passengers
   ↓
Kopar
   ↓
Passengers get off / new passengers board
   ↓
Crowd estimate updated
   ↓
Diva
   ↓
Crowd estimate updated
   ↓
Thane
```

This creates a **dynamic passenger-flow map** rather than relying only on timetable information.

---

## 👥 Adaptive Crowd Tracking

RailPulse uses different levels of tracking depending on crowd conditions.

### Normal Crowd

A whole-train view can be sufficient when passenger density is relatively low.

### Heavy Crowd

During high-density conditions, the train can be divided into **overlapping clusters**, with each cluster covering approximately two coaches.

Each cluster can have **2–3 dynamically selected devices acting as temporary nodes**.

These nodes aggregate information from devices around them and help pass the information toward the train-level communication system.

Node selection can consider:

* Physical position
* Connectivity
* Battery level
* Device stability

Nodes can also change dynamically when required.

The overlapping clusters help maintain coverage and reduce gaps in the crowd estimate.

---

## 📡 Handling Poor Connectivity

Mumbai local trains frequently pass through areas where mobile connectivity can become weak or unavailable.

RailPulse therefore does not depend on every passenger device maintaining continuous internet connectivity.

Journey information can be maintained locally and synchronized when connectivity becomes available.

The goal is for temporary network loss to **degrade the system gracefully rather than completely breaking the crowd model**.

---

## 📊 Estimating Actual Crowd

The number of participating devices does not necessarily equal the number of passengers.

For example:

```text
100 participating devices
        ≠
100 passengers
```

The system can use:

* Observed participating users
* Historical passenger behaviour
* Peak/off-peak patterns
* Participation rates
* Group/family travel patterns
* Statistical buffers

to estimate the **actual passenger population**.

As more journeys are observed, the system can improve its understanding of recurring passenger patterns.

---

## 🔄 Passenger Journey States

A passenger can move through states such as:

```text
At Station
    ↓
Selects Train
    ↓
Boards
    ↓
Travels
    ↓
Reaches Station
    ↓
Gets Off
```

If a passenger misses their selected train, they remain associated with the station rather than incorrectly contributing to that train.

If they board another train, they can transition into that train's journey pool.

The system can also use train movement, location information and journey state to infer what most likely happened when a passenger does not explicitly respond.

A **"waiting for someone"** option can help prevent people who are simply checking train information from being incorrectly counted as intending passengers.

---

## 🗺️ Passenger Flow Map

The main output is more than a single crowd number.

RailPulse aims to create a station-by-station representation of passenger movement:

```text
Dombivli
    │
    ▼
[ Train starts with X passengers ]
    │
    ▼
Kopar
    │
    ├── Passengers get off
    └── Passengers board
    │
    ▼
[ Updated crowd estimate ]
    │
    ▼
Diva
    │
    ├── Passengers get off
    └── Passengers board
    │
    ▼
[ Updated crowd estimate ]
    │
    ▼
Thane
```

Over time, this can form a **dynamic passenger-flow map of the railway network**.

Historical data can further reveal recurring patterns across:

* Trains
* Stations
* Routes
* Time periods
* Passenger flows

---

## 🚦 DETOUR Integration

**DETOUR acts as the decision layer on top of this crowd intelligence.**

The crowd system can provide information such as:

* Current train crowd
* Expected crowd at upcoming stations
* Expected passengers getting off
* Expected passengers boarding
* Train delays
* Alternative services
* Historical passenger-flow patterns

DETOUR can then use this information to help passengers decide whether to:

```text
Take this train
      │
      OR
      ▼
Wait for the next train
      │
      OR
      ▼
Use an alternative service
```

The goal is therefore not simply to suggest alternate routes.

It is to understand **how passengers are distributed and moving through the railway network**, and use that information to make travel decisions more informed.

---

---

## ⚡ Confluent / Kafka Proof of Concept

RailPulse currently has a small proof of concept demonstrating how its real-time passenger-flow events can be represented using **Apache Kafka and Confluent Cloud**.

The current POC uses:

* **Confluent Cloud**
* **Apache Kafka**
* **Datagen Source Connector**
* **Kafka Topic**
* **JSON event schema**
* **Stream Lineage**

### Current Event Flow

```text
RailPulse Datagen Connector
            │
            ▼
     railpulse.events
            │
            ▼
      Kafka Stream

## 🎯 Vision

RailPulse aims to turn participating passenger journeys into a **live, adaptive representation of train occupancy and passenger flow**.

Instead of asking only:

> *"When is my train coming?"*

the system moves toward:

> *"What is the actual state of my train, and what will that state probably look like when it reaches me?"*

---

## One-Line Summary

> **RailPulse turns participating passenger journeys into a real-time, adaptive map of train occupancy and passenger flow across Mumbai's local railway network.**
