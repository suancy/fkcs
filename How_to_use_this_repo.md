# How to Use This Repo
This repo is not for a thorough introduction of [V2Ray](https://v2ray.com) nor for [Docker](https://docs.docker.com/), whose documentations are much more fabulous. Instead, this repo serves as **a description of my solution** for providing anti-censorship services, though in a dumb way.

When you're planning to provide a service, the expected way of distribution and maintenance is:
1. First you build and push the images for the server and client.
2. You distribute a `docker-compose.yml` to clients and tell them to run `docker-compose up`.
3. Now you maintain your server list in the `nginx.conf`, build and push newer images.
4. You would never need to notify your clients because their containers will be automatically updated thanks to [watchtower](https://github.com/v2tec/watchtower)!

## How to setup clients and servers
You should first go through [Get started with Docker](https://docs.docker.com/get-started/) and the [official manual fo V2Ray](https://v2ray.com) before you head to [fkcs_client](./fkcs_client/README.md) and [fkcs_server](./fkcs_server/README.md). There is no code, just configurations, make sure to **read through all the files** carefully.

I'm super willing to help you if you reach me via my **E-mail**: *suancaiyu_at_protonmail.com*, both English and Chinese are welcomed.
