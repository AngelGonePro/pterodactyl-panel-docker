# Pterodactyl Panel + Wings — Docker Compose

Two independent stacks. `pterodactyl-panel/` is fully self-sufficient (bundles its
own MariaDB + Redis, no external DB needed). `pterodactyl-wings/` is separate and
runs on whichever machine(s) will actually host game servers — same box as the
panel or a different one, doesn't matter.

## 1. Deploy the Panel

```
cd pterodactyl-panel
cp .env.example .env
# edit .env: set APP_URL, DB_PASSWORD, DB_ROOT_PASSWORD at minimum
docker compose up -d
```

Wait for it to come up, then create your first admin user:

```
docker compose exec panel php artisan p:user:make
```

## 2. Multi-user

Multi-user support is native to Pterodactyl — nothing extra to configure in
Compose. Once you're logged in as the admin:

- Add more accounts under **Admin → Users** (or run `p:user:make` again for
  each one), and assign the `Root Administrator` flag only to accounts that
  should have full admin access.
- Regular (non-admin) users only see servers you've explicitly assigned to
  them under **Admin → Servers → [server] → Manage → Assign User**, or you
  can add sub-users per-server from within a server's own **Users** tab so
  multiple people can share access to one server without full panel access.

## 3. Create a Location and Node

In the panel: **Admin → Locations** → create one, then **Admin → Nodes** →
create a node pointing at the machine that will run Wings (its FQDN or IP,
whatever the panel will use to reach Wings' port 8080/2022, and whatever
Wings will use to reach the panel back).

Open the new node → **Configuration** tab → the generated YAML has `uuid`,
`token_id`, and `token` fields near the top — that's the only part you need.

## 4. Deploy Wings (on the node machine)

```
cd pterodactyl-wings
cp .env.example .env
```

Edit `.env`: paste in `WINGS_UUID`, `WINGS_TOKEN_ID`, and `WINGS_TOKEN` from
the panel's Configuration tab, set `PANEL_URL`, and for production point the
`PTERODACTYL_*_DIR` vars at persistent absolute paths instead of the `./data`
defaults.

There is no `config.yml` to edit. On every `docker compose up -d`, a small
one-shot `wings-config` container (alpine, since the Wings image itself has
no shell) writes `/etc/pterodactyl/config.yml` from those `.env` values
before the `wings` container is allowed to start. You never touch that file.

```
docker compose up -d
```

That's the whole deployment.

Back in the panel, the node should show a green heart once Wings checks in.
Repeat step 4 on additional machines to add more nodes — each one gets its
own Location/Node entry and its own copy of this `pterodactyl-wings` stack.

## Notes

- Both stacks use their own internal bridge network — no dependency on any
  pre-existing network name, so they'll come up on a fresh VM as-is.
- If the panel sits behind a reverse proxy (nginx/Caddy/Traefik on the host),
  set `TRUSTED_PROXIES` in the panel `.env` to that proxy's IP so Pterodactyl
  sees real client IPs, and point the proxy at `PANEL_HTTP_PORT`.
- Wings needs real access to the host's Docker socket and container storage
  to spin up game server containers — those two bind mounts can't be made
  "portable" the way the rest of the config paths are.
- The generated `config.yml` sets `ignore_panel_config_updates: true` so the
  panel can't overwrite it out from under `.env` on the next restart. If you
  ever want the panel's node Settings page to push config changes directly,
  drop that line from `entrypoint.sh` — but then `.env` stops being the
  source of truth for anything the panel decides to rewrite.
