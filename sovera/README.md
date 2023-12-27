



# Getting Started - Sovera-ix

## Sovera Dify Setup v0.2.0



> This is a fork of [ix](https://github.com/kreneskyp/ix.git) for integration with Sovera Platform. Anything in this branch should be considered as `out-off-tree` from the `ix origin`. 

> :pray:  As always please don't leave any **API KEYs, Tokens, Credentials** or other sensitive information in this file. 

> :reminder_ribbon:  **TODO**
>
> - [ ] Implement pre-commit hook to reject files with sensitive information. 

---



## Dev Deployment

- [ ] **TODO:** write detailed dev documentation. This is too terse for devs without DevOps experience. 



1. Change directory to `docker` and open `docker-compose.yaml` in an editor 

2. Update  `api` configuration block env var `OPENAI_API_BASE:` to point to `internal-llama-gateway` 

3. Run `docker compose up`

4. To run in detached mode run `docker compose up -d` and `docker compose logs -f` to follow output logs

5. Follow instruction [here](./object-store-setup.md) to setup object-store for uploaded files. 

6. 

   

---



## Sovera Dify Setup v0.1.0

This documents contains setup instructions for developing and hosting Dify on Sovera Platform. Configurations are provided for DEV, TEST and PROD environments. 



## DEV

### Install dependencies

You need to install and configure the following dependencies on your machine to build [Dify](https://dify.ai/):

- [Git](http://git-scm.com/)
- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- [Node.js v18.x (LTS)](http://nodejs.org/)
- [npm](https://www.npmjs.com/) version 8.x.x or [Yarn](https://yarnpkg.com/)
- [Python](https://www.python.org/) version 3.10.x



### Install Backend

1. Start the docker-compose stack

   The backend require some middleware, including PostgreSQL, Redis, and Weaviate, which can be started together using `docker-compose`

   ```bash
   cd ../docker
   docker-compose -f docker-compose.middleware.yaml -p dify up -d
   cd ../api
   ```

2. Copy `.env.example` to `.env`

3. Generate a `SECRET_KEY` in the `.env` file.

   ```bash
   openssl rand -base64 42
   ```

   ​	**3.5**  -> If you use annaconda, create a new environment and activate it

   ```
   conda create --name dify python=3.10
   conda activate dify
   ```

4. Install dependencies

   ```
   pip install -r requirements.txt
   ```

5. Run migrate

   Before the first launch, migrate the database to the latest version.

   ```
   flask db upgrade
   ```

6. Start backend:

  ```
  flask run --host 0.0.0.0 --port=5001 --debug
  ```

7. Setup your application by visiting http://localhost:5001/console/api/setup or other apis...

8. If you need to debug local async processing, you can run `celery -A app.celery worker -Q dataset,generation,mail`, celery can do dataset importing and other async tasks.



## Install Frontend

### Getting Started

First, run the development server:

```
npm run dev
# or
yarn dev
# or
pnpm dev
```



- Open [http://localhost:3000](http://localhost:3000/) with your browser to see the result
- Start editing the page by modifying `app/page.tsx`. The page auto-updates as you edit the file




---

### TEST

#### Quick Start Docker AIO Setup

The easiest way to start the Dify server on sovera platform  is to run our [docker-compose.yml](https://github.com/langgenius/dify/blob/main/docker/docker-compose.yaml) file. Before running the installation command, make sure that [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/) are installed on your machine:

> 👉 Images used for API and Web-Server services in the docker-compose file are generated for Sovera Platform. They include some modified files. See below for how to build docker images and a list of modified files. 

```bash
cd docker
docker compose up -d
```

### Helm Kubernetes AIO Setup

```bash
helm repo add dify https://borispolonsky.github.io/dify-helm
helm repo update
helm install my-release dify/dify
```

### Configuration

If you need to customize the configuration, please refer to the comments in our [docker-compose.yml](https://github.com/langgenius/dify/blob/main/docker/docker-compose.yaml) file and manually set the environment configuration. After making the changes, please run `docker-compose up -d` again

---

## PROD

`updated: Oct 10, 2023`

#### Work-in-progress
> This is probably outdated 

### K8S Deployment
- all the individuals `k8s` manifests are in `/k8s` folder under `docker` directory
- So far it works on mini-kube and kind
- TODO: create `kustomize` package -> Create `kubevela` OAM app package

---

### Build Docker Image

#### API Server

```shell
cd api
docker build -t kloudtaxi/sovera-dify-api:0.3.12 .
```

#### Webserver

```
cd web
docker build -t kloudtaxi/sovera-dify-web:0.3.12 .
```

---



## Modifications 

```bash
	new file:   .sovera/dify-setup.md
	new file:   .sovera/object-store-setup.md
	modified:   api/Dockerfile
	modified:   api/config.py
	modified:   docker/docker-compose.middleware.yaml
	modified:   docker/docker-compose.yaml
	modified:   docker/nginx/conf.d/default.conf
	new file:   web/.dockerignore
	modified:   web/Dockerfile
	modified:   web/app/(commonLayout)/apps/page.tsx
	modified:   web/app/(commonLayout)/datasets/DatasetFooter.tsx
	modified:   web/app/(commonLayout)/layout.tsx
	modified:   web/app/components/app/configuration/debug/index.tsx
	modified:   web/app/components/header/index.module.css
	modified:   web/app/components/header/index.tsx
	new file:   web/app/components/share/chat/welcome/icons/logo-original.png
	modified:   web/app/components/share/chat/welcome/icons/logo.png
	modified:   web/config/index.ts
	new file:   web/public/favicon-original.ico
	modified:   web/public/favicon.ico
	modified:   web/tailwind.config.js
```

