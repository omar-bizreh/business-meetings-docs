# Business Meetings

## Overview

This Project is an open-source project written in `Typescript` and uses `ExpressJs` & `ReactJs` and uses `MongoDb` storing meeting data

Currently it includes the following features:

- Parnters
  - List partners
  - View partner info
  - Invite to meeting
- Meetings
  - View meetings
  - Create new one

## Getting Started

Start by reading docs for the language of your choice

### Meetings API

- [Docs](/meetings/dependencies/?id=overview)
- [Source Code](https://github.com/SudoDevOSS/business-meeting-meetings-api)

### Partners API

- [Docs](/partners/dependencies/?id=overview)
- [Source Code](https://github.com/SudoDevOSS/business-meeting-partners-api)

### Dashboard

- [Docs](/dashboard/dependencies/?id=overview)
- [Source Code](https://github.com/SudoDevOSS/business-meeting-frontend)

## Deployment

### Docker

Source code includes docker file for each service ready for building container with release build.

#### Prebuilt

If you wish you to use prebuilt containers:

- [Meetings API Container](https://hub.docker.com/r/sudodevosss/meetings-api)
- [Partners API Container](https://hub.docker.com/r/sudodevosss/partners-api)
- [Dashboard Container](https://hub.docker.com/r/sudodevosss/meetings-dashboard)

Make sure you check each API publishing section for any required environment variables for the container

### Building your own containers

Each project includes its own `Dockerfile` than can be built by simply exeucing this command in project root:

`docker build -t USER/APP_NAME:RELEASE .`

### Docker Compose

If you wish to use Docker Compose, you can do so by using the configuration below:

```yml
version: '3.4'

services:
  api-gateway:
    image: meetings.api.gateway:r1
    build: ./api-gateway/
    ports:
      - 127.0.0.1:8080:8080
    networks:
      - frontend
      - backend
  meetings-api:
    image: meetings.api:r1
    build: ./meetings-api/
    environment:
      DB_SOURCE: mongodb://db
      DB_NAME: meetingsdb
      PORT: 3032
    networks:
      - backend
    depends_on:
      - db
    links:
      - db
    restart: on-failure
  partners-api:
    image: partners.api:r1
    build: ./partners-api/
    environment:
      PORT: 3030
    networks:
      - backend
  frontend:
    image: meetings.website:r1
    build: ./website/
    ports:
      - 127.0.0.01:5000:5000
    networks:
      - frontend
  db:
    image: mongo:latest
    ports:
      - 27017
    networks:
      - backend
    volumes:
      - mongodata:/data/db
volumes:
  mongodata:

networks:
  frontend:
    driver: bridge
  backend:
    driver: bridge
```

## License

```text

MIT License

Copyright (c) 2021 sudo-dev-oss

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```