# Kafka Location Sharing Demo

A small Node.js demo that streams browser location updates through Kafka and broadcasts them to connected clients with Socket.IO.

The app includes:

- An Express server that serves the map UI from `public/index.html`
- A Socket.IO connection for sending and receiving location updates
- A Kafka producer that publishes location updates to the `location-updates` topic
- Kafka consumers that read updates and broadcast/log them
- A Docker Compose setup for running Kafka locally

## Tech Stack

- Node.js
- Express
- Socket.IO
- KafkaJS
- Apache Kafka
- Leaflet.js
- Docker Compose
- pnpm

## Project Structure

```text
.
|-- database-processor.js   # Kafka consumer that logs location updates
|-- docker-compose.yml      # Local Kafka broker setup
|-- index.js                # Express + Socket.IO server and Kafka producer/consumer
|-- kafka-admin.js          # Creates the Kafka topic
|-- kafka-client.js         # Shared KafkaJS client
|-- package.json
`-- public/
    `-- index.html          # Browser map UI
```

## Prerequisites

Make sure you have these installed:

- Node.js
- pnpm
- Docker Desktop

## Setup

Install dependencies:

```bash
pnpm install
```

Start Kafka:

```bash
docker compose up -d
```

Create the Kafka topic:

```bash
node kafka-admin.js
```

The topic created is:

```text
location-updates
```

## Running the App

Start the main server:

```bash
node index.js
```

By default, the server runs at:

```text
http://localhost:8000
```

Open that URL in your browser. The page asks for location permission and sends your location every 10 seconds.

To run the separate database processor consumer:

```bash
node database-processor.js
```

This consumer reads messages from Kafka and logs them as a placeholder for inserting location data into a database.

## How It Works

1. The browser gets the user's current location with the Geolocation API.
2. The browser sends the location to the server with Socket.IO.
3. The server publishes the location to Kafka on the `location-updates` topic.
4. Kafka consumers read the location updates.
5. The main server broadcasts updates back to connected clients.
6. The browser updates markers on the Leaflet map.

## Useful Commands

Check the server health:

```bash
curl http://localhost:8000/health
```

Stop Kafka:

```bash
docker compose down
```

## Notes

- Kafka is configured to run on `localhost:9092`.
- The browser must allow location access for live updates to work.
- The map tiles are loaded from OpenStreetMap.
- `database-processor.js` currently logs messages instead of writing to a real database.
