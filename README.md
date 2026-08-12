```
# Copy and edit .env
cp ~/pterodactyl-panel/.env.example ~/pterodactyl-panel/.env
nano ~/pterodactyl-panel/.env
```

```
mkdir pterodactyl-panel && \
curl -L -o /tmp/pterodactyl-panel.zip https://raw.githubusercontent.com/AngelGonePro/pterodactyl-panel-docker/refs/heads/main/pterodactyl-panel.zip && \
python3 - << 'EOF'
import zipfile, os
zip_path = "/tmp/pterodactyl-panel.zip"
extract_to = "pterodactyl-panel"

with zipfile.ZipFile(zip_path) as z:
    for member in z.namelist():
        parts = member.split("/", 1)
        if len(parts) > 1:
            target = os.path.join(extract_to, parts[1])
            if not member.endswith("/"):
                os.makedirs(os.path.dirname(target), exist_ok=True)
                with open(target, "wb") as f:
                    f.write(z.read(member))
EOF
rm /tmp/pterodactyl-panel.zip
```

```
# Copy and edit .env
cp ~/pterodactyl-wings/.env.example ~/pterodactyl-wings/.env
nano ~/pterodactyl-wings/.env
```
```
# Copy and edit .env
cp ~/pterodactyl-wings/.env.example ~/pterodactyl-wings/.env
nano ~/pterodactyl-wings/.env
```

```
mkdir pterodactyl-wings && \
curl -L -o /tmp/pterodactyl-panel.zip https://raw.githubusercontent.com/AngelGonePro/pterodactyl-panel-docker/refs/heads/main/pterodactyl-wings.zip && \
python3 - << 'EOF'
import zipfile, os
zip_path = "/tmp/pterodactyl-wings.zip"
extract_to = "pterodactyl-wings"

with zipfile.ZipFile(zip_path) as z:
    for member in z.namelist():
        parts = member.split("/", 1)
        if len(parts) > 1:
            target = os.path.join(extract_to, parts[1])
            if not member.endswith("/"):
                os.makedirs(os.path.dirname(target), exist_ok=True)
                with open(target, "wb") as f:
                    f.write(z.read(member))
EOF
rm /tmp/pterodactyl-wings.zip
```
