# Get started

- [With Docker Compose](#with-docker-compose)
- [With Docker](#with-docker)
- [Without Docker](#without-docker)
- [With Heroku](#with-heroku)

## With Docker Compose

### 1. Create the configuration

Create a [`.env` file](https://www.npmjs.com/package/dotenv) in the root of this project to store all environment variables in one file.

```
ACKEE_USERNAME=username
ACKEE_PASSWORD=password
```

Only those two are required to run Ackee with the existing `docker-compose.yml`. Take a look at the [options](Options.md) for a detailed explanation.

### 2. Run Ackee

Run this command in the root of the project to use the predefined `docker-compose.yml`. It contains everything you need, including MongoDB and Ackee.

```
docker-compose up
```

> 💡 Add the `-d` flag to the Docker command to run the services in the background.

### 3. Open Ackee

Ackee will output the URL it's listening on once the server is running. Visit the URL with your browser and complete the finial steps using the interface.

### 4. Get Ackee online

Ackee now runs on port `3000` and is only accessible from you local network. It's recommended to use a reverse proxy in front of Ackee. The following guides will help you through this steps.

- [SSL and HTTPS](SSL%20and%20HTTPS.md)
- [CORS headers](CORS%20headers.md)

## With Docker

### 1. Setup MongoDB

Ackee requires a running MongoDB instance. The easiest way to install MongoDB is by using [Docker](https://www.docker.com). Skip this step if you have MongoDB installed or visit the [website of MongoDB](https://www.mongodb.com) for alternative setups.

```
docker run -p 27017:27017 --name mongo mongo
```

For persistent storage, mount a host directory to the container directory `/data/db`, which is identified as a potential mount point in the mongo Dockerfile. When starting a new container, Docker will use the volume of the previous container and copy it to the new container, ensuring that no data gets lost.

```
docker run -p 27017:27017 -v /path/to/local/folder:/data/db --name mongo mongo
```

> 💡 Add the `-d` flag to the Docker command to run MongoDB in the background.

Explanation:

- `-p` makes port `27017` available at port `27017` on the host
- `-v` mounts `/path/to/local/folder` to `/data/db` of the container
- `--name` sets the container name to `mongo`
- `mongo` is the name of the image

### 2. Run Ackee

```
docker run -p 3000:3000 -e ACKEE_MONGODB='mongodb://mongo:27017/ackee' -e ACKEE_USERNAME='username' -e ACKEE_PASSWORD='password' --link mongo --name ackee electerious/ackee
```

> 💡 Add the `-d` flag to the Docker command to run Ackee in the background.

Explanation:

- `-p` makes port `3000` available at port `3000` on the host
- `-e` sets [environment variables](Options.md) required by Ackee
- `--link` links Ackee with the `mongo` container
- `--name` sets the container name to `ackee`
- `electerious/ackee` is the name of the image

### 3. Open Ackee

Ackee will output the URL it's listening on once the server is running. Visit the URL with your browser and complete the finial steps using the interface.

### 4. Get Ackee online

Ackee now runs on port `3000` and is only accessible from you local network. It's recommended to use a reverse proxy in front of Ackee. The following guides will help you through this steps.

- [SSL and HTTPS](SSL%20and%20HTTPS.md)
- [CORS headers](CORS%20headers.md)

## Without Docker

### 1. Install dependencies

Ackee dependents on …

- [Node.js](https://nodejs.org/en/) (v10.16 or newer)
- [yarn](https://yarnpkg.com/en/)
- [MongoDB](https://www.mongodb.com) (v4.0.6 or newer)

Make sure to install and update all dependencies before you continue. The installation instructions for the individual dependencies can be found on the linked websites.

### 2. Create the configuration

Configure Ackee using environment variables or create a [`.env` file](https://www.npmjs.com/package/dotenv) in the root of the project to store all variables in one file.

```
ACKEE_MONGODB=mongodb://localhost:27017/ackee
ACKEE_USERNAME=username
ACKEE_PASSWORD=password
```

Only those three are required to run Ackee. Take a look at the [options](Options.md) for a detailed explanation.

The [MongoDB connection string](https://docs.mongodb.com/manual/reference/connection-string/) is used to connect to your MongoDB. It should also contain the username and password of your MongoDB instance when required.

The username and password variables are used to secure your Ackee interface/API.

### 3. Install Ackee

Install all required dependencies.

```
yarn
```

### 4. Run Ackee

Ackee will output the URL it's listening on once the server is running. Visit the URL with your browser and complete the finial steps using the interface.

```
yarn start
```

### 5. Get Ackee online

Ackee now runs on port `3000` and is only accessible from you local network. It's recommended to use a reverse proxy in front of Ackee. The following guides will help you through this steps.

- [SSL and HTTPS](SSL%20and%20HTTPS.md)
- [CORS headers](CORS%20headers.md)

## With Heroku

### 1. Deploy to Heroku

Simply deploy to Heroku by clicking this button:

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/electerious/Ackee)

### 2. Configure Ackee

Ensure that you're using the correct CORS headers by setting [`ACKEE_ALLOW_ORIGIN`](CORS%20headers.md#heroku-or-platforms-as-a-service-configuration).