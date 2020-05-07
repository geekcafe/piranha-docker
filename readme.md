# Piranha Docker

This repo contains a quick and easy way to run the piranha cms via docker container.

The root docker-compose file will run the Piranha WebRazor .net core app behind an ngnix container


## Quick start w/ nginx

Run the following commands

`docker-compose build`

`docker-compose up` or `docker-compose up -d`

Navigate to `http://localhost:8181`


## Quick start w/ just .net core (kestrel)

If you want to run the .net core outside of the nginx container, you can run the docker-compose file from within the piranha-razor directory

 `cd piranha-razor`

 `docker-compose build`

`docker-compose up -d`

Navigate to `http://localhost:5000`


## Docker Build Infomration

The `docker-compose build` command will clone the latest piranha cms repo, build it, package it and deploy it into another container.  Each container has the appropriate build tools, so you don't need to install .net core.  You just need git.


## Data Storage & Data Volatility

The default database in the RazorWeb project uses a local Sqlite db.  If you destroy the container your site will be destroyed.  This isn't for production use.  I'll be adding a MySql version soon.


### Useful single commands

`docker-compose up -d --build`


