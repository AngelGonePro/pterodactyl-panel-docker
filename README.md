```
# Copy and edit .env
cp ~/pterodactyl-panel/.env.example ~/pterodactyl-panel/.env
nano ~/linode-nginx-stack/.env
```

```
mkdir linode-nginx-stack && \
curl -L -o /tmp/vpn.zip https://raw.githubusercontent.com/AngelGonePro/pterodactyl-panel-docker/refs/heads/main/pterodactyl-panel.zip && \
python3 - << 'EOF'
import zipfile, os
zip_path = "/tmp/vpn.zip"
extract_to = "linode-nginx-stack"

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
rm /tmp/vpn.zip
```

```
# Copy and edit .env
cp ~/pterodactyl-panel/.env.example ~/pterodactyl-panel/.env
nano ~/linode-nginx-stack/.env
```

```
mkdir linode-nginx-stack && \
curl -L -o /tmp/vpn.zip https://raw.githubusercontent.com/AngelGonePro/pterodactyl-panel-docker/refs/heads/main/pterodactyl-panel.zip && \
python3 - << 'EOF'
import zipfile, os
zip_path = "/tmp/vpn.zip"
extract_to = "linode-nginx-stack"

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
rm /tmp/vpn.zip
```
