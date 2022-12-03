# Devopsdocker

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 12.0.5.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via a platform of your choice. To use this command, you need to first add a package that implements end-to-end testing capabilities.

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI Overview and Command Reference](https://angular.io/cli) page.
## Creating Dockerfile

Now let's create the Dockerfile which will provide the instructions in creating our container_aitousha container.
# implement a multi-stage build
# 1ST STAGE: Node image for building Angular assets (dist files)

# name the node image "builder"
FROM node:14.17.3 AS builder

# set working directory
WORKDIR /app

# copy all files from current dir to working dir in image
COPY . .

# install node modules and build assets 
RUN npm i && npm run build

# 2ND STAGE: Nginx to serve Angular assets built from previous stage

# nginx state for serving content
FROM nginx:latest

# Set working directory to nginx asset directory
WORKDIR /usr/share/nginx/html

# Remove default nginx static assets
RUN rm -rf ./*

# copy Angular static assets (dist files) from builder image
COPY --from=builder ./app/dist/devopsdocker /usr/share/nginx/html

# Container run nginx 
# -g  global directives
# daemon off
ENTRYPOINT ["nginx", "-g", "daemon off;"]
## Create nginx.conf file

The nginx.conf will contain the configuration for our Nginx server.

Place the configuration file inside

aitoucha/docker/nginx/
server {
 listen 80;
 server_name web;
 location / {
   # This would be the directory where your Web app's static files are stored at
   root /usr/share/nginx/html;
   try_files $uri /index.html;
 }

}
Let's assume the app's API server is running at port 8081. Any request having a path of /api/v1/ will be redirected to our API server running at localhost:8081.
## Run the Docker Nginx Server
docker run --name container_aitousha -p  8081:80 aitoucha:v1 

After Docker finishes building, your Nginx server will be running.

Go to your domain and enjoy the sight of your website being served by Nginx.

Stay stoked and code. :)