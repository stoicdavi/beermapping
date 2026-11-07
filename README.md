# Beermapping

Beermapping is a small Node.js and Express service for serving beer-location data from local XML files. The repository includes city data in `data/`, and the service exposes that data over HTTP. It is designed to provide a simple local API rather than fetch data on each request.

## API

The server listens on port `3000`.

- `GET /locations` returns the configured list of city names as JSON.
- `GET /locations/:place` returns the matching XML file from `data/`. If no matching file is found, the service currently returns `data/mikkeli.xml` as a fallback.

For example, `GET http://localhost:3000/locations/turku` serves `data/turku.xml`.

## Run locally

Requires Node.js and npm.

```sh
npm install
node index.js
```

Then visit `http://localhost:3000/locations` or request a city with `/locations/:place`.

## Run with Docker

Build and start the container, publishing its port 3000:

```sh
docker build -t beermapping .
docker run --rm -p 3000:3000 beermapping
```

## Data and deployment

City XML files are stored in the `data/` directory and served directly by the app. The source also contains a routine for downloading city data from the BeerMapping web service, but that routine is not run by default.

The GitHub Actions workflow builds and pushes a Docker image tagged `latest` when changes are pushed to `main`. It expects the repository's `DOCKER_USERNAME` and `DOCKER_PASSWORD` secrets to be configured for Docker Hub login.
