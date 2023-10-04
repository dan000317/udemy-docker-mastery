# Assignment: Create A Multi-Service Multi-Node Web App

## Goal: create networks, volumes, and services for a web-based "cats vs. dogs" voting app

Here is a basic diagram of how the 5 services will work:

![diagram](./architecture.png)

- All images are on Docker Hub, so you should use editor to craft your commands locally,
then paste them into swarm shell (at least that's how I'd do it)
- a `backend` and `frontend` overlay network are needed.
Nothing different about them other than that backend will help protect database from the voting web app.
(similar to how a VLAN setup might be in traditional architecture)
- The database server should use a named volume for preserving data.
Use the new `--mount` format to do this: `--mount type=volume,source=db-data,target=/var/lib/postgresql/data`

### Services (names below should be service names)

- vote
  - bretfisher/examplevotingapp_vote xxxxxxxxxxxxxxx
  - web frontend for users to vote dog/cat
  - ideally published on TCP 80. Container listens on 80 xxxxxxxx
  - on frontend network xxxxxxxx
  - 2+ replicas of this container xxxxxxxxxxxxx

- redis
  - redis:3.2 xxxxxxxxxxx
  - key-value storage for incoming votes
  - no public ports xxxxxx
  - on frontend network xxxxxxxxx
  - 1 replica NOTE VIDEO SAYS TWO BUT ONLY ONE NEEDED xxxxxx

- worker
  - bretfisher/examplevotingapp_worker xxxxxxxxxx
  - backend processor of redis and storing results in postgres
  - no public ports xxxxxxxxxx
  - on frontend and backend networks xxxxxxxxxxx
  - 1 replica xxxxxxxxxxx

- db
  - postgres:9.4 xxxxxxxxxxx
  - one named volume needed, pointing to /var/lib/postgresql/data xxxxxxxxxx
  - on backend network xxxxxxxxxx
  - 1 replica xxxxxxxxxxx
  - remember set env for password-less connections -e POSTGRES_HOST_AUTH_METHOD=trust xxxxxxxxxxxxx

- result
  - bretfisher/examplevotingapp_result xxxxxxxxxx
  - web app that shows results xxxxxxxxxx
  - runs on high port since just for admins (lets imagine) xxxxxxxxxxxx
  - so run on a high port of your choosing (I choose 5001), container listens on 80 xxxxxxxx
  - on backend network xxxxxxxxx
  - 1 replica xxxxxxxxxxxx
