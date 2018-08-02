# Server Setups
## Modify `config.json`
Please modify the configuration file under `myconf` before use.

### TLS
I use the simple combination of tcp-on-tls of V2Ray, with the port opened on `443`. You have to generate a self-certified key pair for tls encryption. One way to generate the key pair is through `v2ctl`, a binary executive which you can find in recent releases of [v2ray core](https://github.com/v2ray/v2ray-core/releases).
```shell
$ v2ctl cert -ca
```
Insert the generated key pair to `config.json` under `myconf`.

### VMESS
In this setup, clients are expected to establish connections with servers through `vmess`, this requires both parties have system times differ no more than 2 minutes (UTC times). Also you'll need a `UUID` (yuo can generate it [here](https://www.uuidgenerator.net/)) as a client's id. Must be identical on the server and client.

## Run
Securely copy `myconf` and `docker-compose.yml` to your server, and run `docker-compose up`, your container will be updated automatically when a new core of v2ray is released.
