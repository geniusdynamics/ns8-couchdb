📘 How to Use CouchDB in NethServer 8
This guide explains how to deploy, configure, and manage a CouchDB instance using the ns8-couchdb module on NethServer 8.

🚀 1. Install the CouchDB Module
To install the latest CouchDB module from GitHub Container Registry:

```bash

add-module ghcr.io/geniusdynamics/couchdb:latest 1
```

The command returns a JSON object with the instance name, such as:

```json
{
  "module_id": "couchdb1",
  "image_name": "couchdb",
  "image_url": "ghcr.io/nethserver/couchdb:latest"
}
```

⚙️ 2. Configure the CouchDB Instance
To configure the instance (e.g., set up virtual host, enable HTTPS, etc.), run:

```bash
api-cli run configure-module --agent module/couchdb1 --data - <<EOF
{
"host": "couchdb.example.com",
"http2https": true,
"lets_encrypt": true
}
```

This:

Starts and configures the CouchDB service

Sets up Traefik with your specified domain

Note: Replace couchdb.example.com with your actual domain name.

🧾 3. Retrieve Current Configuration
To view the current configuration of the CouchDB instance:

```bash
api-cli run get-configuration --agent module/couchdb1
```

🔄 4. Update the CouchDB Module
To force an update of the module:

```bash
api-cli run update-module --data '{
"module_url": "ghcr.io/geniusdynamics/couchdb:latest",
"instances": ["couchdb1"],
"force": true
}'
```

🗑️ 5. Uninstall the CouchDB Instance
To completely remove a CouchDB instance (including data):

```bash
remove-module --no-preserve couchdb1
```

📨 6. Smarthost Integration
The module auto-discovers smarthost settings from the central Redis configuration on start-up:

It runs bin/discover-smarthost to refresh state/smarthost.env

If the smarthost configuration changes while CouchDB is running, the event smarthost-changed will automatically restart the service

For more details, see:

events/smarthost-changed/10reload_services

systemd/user/couchdb.service

🧪 7. Debugging Tools
✅ View environment variables used by the agent:

```bash
runagent -m couchdb1 env
```

✅ Become the runagent shell user (loads full environment):

```bash
runagent -m couchdb1
```

## Accessing CouchDB

1. Accessing CouchDB via Web UI (Fauxton)

[Fauxton](https://couchdb.apache.org/#download)
