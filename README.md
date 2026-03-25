# Docker Compose exercise: two isolated WordPress environments behind Nginx

## Goal

Starting from the provided `docker-compose.yml`, modify the stack so it runs:

- **2 WordPress containers**
- **2 MariaDB containers**
- **1 Nginx container** acting as the single entry point

All services must live in **one single `docker-compose.yml` file**.

The final result should allow exposing both WordPress sites through Nginx, while keeping the two application environments **independent from each other**.

## Starting point

This repository includes a basic WordPress + MariaDB stack.

Your task is to evolve it into a setup with two separate WordPress installations.

## Requirements

Your final compose file must meet these conditions:

1. There must be **two different WordPress instances**.
2. Each WordPress instance must use **its own database**.
3. There must be **one Nginx container** in front of both WordPress instances.
4. Nginx must be the **only service exposed to the host**.
5. The architecture must use **networks** so that:
   - `wordpress1` can reach only `db1`
   - `wordpress2` can reach only `db2`
   - the two WordPress environments are isolated from each other
6. Everything must be defined in **a single compose file**.

## Suggested access model

You can choose one of these approaches:

### Option A: different paths

- `http://localhost/site1`
- `http://localhost/site2`

### Option B: different hostnames

- `site1.localhost`
- `site2.localhost`

If you choose hostnames, remember to document how to test them.

## Hints

- You will likely need **two custom networks** for the isolated environments.
- Nginx may need to be connected to **both networks** so it can reach both WordPress containers.
- Each WordPress must point to its own database service using the corresponding Compose service name.
- Only Nginx should publish a port such as `80:80`.
- You may use named volumes for persistence.

## Expected architecture

An example logical design would look like this:

- `nginx` connected to `site1_net` and `site2_net`
- `wordpress1` connected to `site1_net`
- `db1` connected to `site1_net`
- `wordpress2` connected to `site2_net`
- `db2` connected to `site2_net`

## Deliverables

At the end of the exercise, students should provide:

- the updated `docker-compose.yml`
- any extra Nginx configuration needed
- brief instructions to run and test the stack

## How to start

Run the initial stack with:

```bash
docker compose up -d
```

Then open:

```text
http://localhost:8080
```

## Validation checklist

Use this checklist to verify your solution:

- [ ] `docker compose up -d` starts the full stack
- [ ] only Nginx is exposed to the host
- [ ] both WordPress instances are reachable through Nginx
- [ ] WordPress 1 uses only database 1
- [ ] WordPress 2 uses only database 2
- [ ] WordPress 1 cannot directly reach database 2
- [ ] WordPress 2 cannot directly reach database 1
- [ ] everything is defined from a single compose file

## Optional extra challenges

- Add healthchecks
- Add persistent volumes for both databases and both WordPress instances
- Make Nginx route by hostname instead of path
- Add a custom home page in Nginx linking to both sites

## Help

```
      WORDPRESS_CONFIG_EXTRA: |
        define('WP_HOME', 'http://localhost/site2');
        define('WP_SITEURL', 'http://localhost/site2');
        if (isset($$_SERVER['HTTP_X_FORWARDED_PROTO']) && $$_SERVER['HTTP_X_FORWARDED_PROTO'] === 'https') {
          $$_SERVER['HTTPS'] = 'on';
```
