```
docker compose exec panel php artisan p:user:make
```

```
# absolute paths instead of the ./data defaults
mkdir -p data/etc-pterodactyl data/var-lib-pterodactyl data/var-log-pterodactyl data/tmp-pterodactyl
```

```
cd ~/pterodactyl-wings && mv docker-compose.yml docker-compose.yml.bak && mv entrypoint.sh entrypoint.sh.bak && curl -fsSL -o /tmp/dl.zip https://github.com/AngelGonePro/pterodactyl-panel-docker/raw/refs/heads/main/pterodactyl-wings.zip && python3 -c "import zipfile; open('deploy-update.sh','wb').write(zipfile.ZipFile('/tmp/dl.zip').read('pterodactyl-wings/deploy-update.sh'))" && chmod +x deploy-update.sh && ./deploy-update.sh https://github.com/AngelGonePro/pterodactyl-panel-docker/raw/refs/heads/main/pterodactyl-wings.zip . && docker compose up -d --remove-orphans
```

```
cd ~/pterodactyl-panel && mv docker-compose.yml docker-compose.yml.bak && curl -fsSL -o /tmp/dl.zip https://github.com/AngelGonePro/pterodactyl-panel-docker/raw/refs/heads/main/pterodactyl-panel.zip && python3 -c "import zipfile; open('deploy-update.sh','wb').write(zipfile.ZipFile('/tmp/dl.zip').read('pterodactyl-panel/deploy-update.sh'))" && chmod +x deploy-update.sh && ./deploy-update.sh https://github.com/AngelGonePro/pterodactyl-panel-docker/raw/refs/heads/main/pterodactyl-panel.zip . && docker compose pull && docker compose up -d --remove-orphans
```
