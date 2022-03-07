Dockerfile Guantlet Challenge.


1. volumes:
    - "./app:/usr/src/app/app:cached"

    Using "cached" relaxes consistency guarantees.  Fine for development, but for a production application
    it can be a disaster.

2. web:
    environment:
      - SECRET_KEY="BJAJLDJ38439202jgal*lam4m43lajgjgfah"

    Putting a secret key directly into a docker-compose file is BAD.   Alternatives would be to 
    put it into a .env file not tracked in GIT, or leave off the value to have it sourced from 
    the environment *running* docker-compose.

3. depends_on:
    - waiting for one thing to be up before another is, can be a sign of brittle architecture.  
      it shouldn't be needed in production.

4. Make some code in a container that causes the container to just reboot over and over again.  Source   
    the cause of the error in the logs.

5. Under provision a DB, not giving it enough memory to work.  Looks like a typo 8M instead of 8G
6. Binds to a port on the production host that is already in USE.
7. - Mounts a volume to code, making it available outside the container.. not a best practice.
8. - restart: always (recommended for production)
9. - leaving debugging and verbose logging on

10. - omitting a version: tag in the yaml indicates that it's using docker-compose v1 specification which is deprecated.

Compose does not take advantage of networking when you use version 1: every container is placed on the default bridge network and is reachable from every other container at its IP address. You need to use links to enable discovery between containers.

11. - uses expose when they are meant to use bind.   i.e. the port is published on the container, but not brought through to the host.
12. - code attempts to mount a directory into it that doesn't exist!
service: build.
13. Unquoted numerical values for CPU's

Scenario:

You have just received a Pull Request review from one of the developers on the team.  They have been 
working on some code in a test environment for months, updating some functionality for a pretty critical
application.   Since you are DevOps, they asked that you have a look particularly at their docker-compose 
file and suggest any fixes or edits.

The user openly admitted they are brand new to Docker, and Docker-compose, and may not have gotten things right.

Once approved, this PR will roll out automatically into production.  Please be thorough with your review!
