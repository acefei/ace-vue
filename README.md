# Project Title

A brief description of your project.

## Table of Contents

- [Project Title](#project-title)
- [Description](#description)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgments](#acknowledgments)

## Description

A Practice Vue Project with Vuetify + Pinia + TS

## Getting Started

### Prerequisites
- pnpm (curl -fsSL https://get.pnpm.io/install.sh | sh -)

### Create Vue Project
```
$ pnpm create vuetify
... 
✔ Project name: … vuetify-project
✔ Which preset would you like to install? › Custom (Choose your features)
✔ Use TypeScript? … No / Yes
✔ Use Vue Router? … No / Yes
✔ Use Pinia? … No / Yes
✔ Use ESLint? … No / Yes
✔ Would you like to install dependencies with yarn, npm, pnpm, or bun? › pnpm
```


### Run Project with Docker
Create Dockerfile along with package.json
```
FROM node:20-slim AS base
ENV PNPM_HOME="/pnpm"
ENV PATH="$PNPM_HOME:$PATH"
RUN corepack enable
COPY . /app
WORKDIR /app

FROM base AS prod-deps
RUN --mount=type=cache,id=pnpm,target=/pnpm/store pnpm install --prod --frozen-lockfile

FROM base AS build
RUN --mount=type=cache,id=pnpm,target=/pnpm/store pnpm install --frozen-lockfile
RUN pnpm run build

FROM nginx:latest
COPY --from=build /app/dist /usr/share/nginx/html
```

Create .dockerignore
```
node_modules
.git
.gitignore
*.md
dist
```

Build and Run Docker
```
$ docker build . -t vue-app
$ docker run -p 8000:80 vue-app
```