# Client Setups
## Modify `config.json` and `nginx.conf`
Please modify the configuration files under `myconf` before use.

### NGINX load balancing
The configuration comes from [使用 Nginx 对 Project V 做负载均衡](https://ellinia.me/Load-balancing-project-v-by-nginx/). You'll have to change the list of servers to your own.

### VMESS
You'll need a `UUID` (yuo can ganerate it [here](https://www.uuidgenerator.net/)) as the client's id which should be identical with the client's id on your servers.

## Build the image
Follow the [official docs](https://docs.docker.com/get-started/) carefully when you build and push your image.
