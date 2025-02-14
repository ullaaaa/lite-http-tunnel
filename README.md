# Lite HTTP Tunnel

A tunnel tool to help you expose local web (HTTP/WebSocket) server behind a NAT or firewall to the internet. Inspired by [Ngrok](https://github.com/inconshreveable/ngrok) and [node-http-proxy](https://github.com/http-party/node-http-proxy).

![http tunnel](https://user-images.githubusercontent.com/7036536/155876708-f30f4921-c8c8-463d-8917-c4f932d3b2e6.png)

## How it work

The tunnel is based on `WebSocket`. We have a `WebSocket` connection between the client and server to stream HTTP/WebSocket requests from public server to your local server.

## Usage

### Deploy at public server

Firstly please deploy this project to your own web host with public internet access. The project is just a `Node.js` web server based on `Express.js`. So just deploy as what you do for [deploying Node.js web server](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/deployment).

You can deploy with Heroku with `Deploy To Heroku` button below:

[![Deploy To Heroku](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

With Heroku, you can get a free HTTPS web server with a fixed public domain.

#### Get JWT Token

In first deployment, you need to provide `JWT_GENERATOR_USERNAME` and `JWT_GENERATOR_PASSWORD` environment variables, then you can access `https://your_web_host_domain/tunnel_jwt_generator?username=your_generator_username&password=your_generator_password` to get `JWT Token`. After you get `JWT Token`, you can remove `JWT_GENERATOR_USERNAME` and `JWT_GENERATOR_PASSWORD` environment variables to remove `/tunnel_jwt_generator` access.

You can also generate JWT Token in your local by following code [here](https://github.com/embbnux/lite-http-tunnel/blob/main/server.js#L71).

### Setup Client

#### Install client

Please install `lite-http-tunnel` client in your local computer where it can access your local HTTP server.

```shell
$ npm i -g lite-http-tunnel
$ lite-http-tunnel -h
```

#### Config remote public server address:

```shell
$ lite-http-tunnel config server https://your_web_host_domain
```

#### Config jwt token that you got from server:

```shell
$ lite-http-tunnel config jwt your_jwt_token
```

#### Or With specified profile

```shell
$ lite-http-tunnel config server https://your_web_host_domain -p profile1
$ lite-http-tunnel config jwt your_jwt_token -p profile1
```

#### Start client

```shell
$ lite-http-tunnel start your_local_server_port
```

Please replace your_local_server_port with your local HTTP server port, eg: `8080`.

After that you can access your local HTTP server by access `your_public_server_domain`.

#### Start with specified profile:

```shell
$ lite-http-tunnel start your_local_server_port -p profile1
```

#### Change origin to local server:

```shell
$ lite-http-tunnel start your_local_server_port -o localhost:5000
```

#### Change local server host:

```shell
$ lite-http-tunnel start your_local_server_port -h localhost1
```

## Multiple Clients

The server steams web request to WebSocket connection which has same host value in request headers.

So if you have multiple domains for the proxy server, you can have multiple clients.

For example, you have `https://app1.test.com` and `https://app2.test.com` for this proxy server.

In client 1:

```
$ lite-http-tunnel config server https://app1.test.com -p profile1
$ lite-http-tunnel start your_local_server_port -p profile1
```

In client 2:

```
$ lite-http-tunnel config server https://app2.test.com -p profile2
$ lite-http-tunnel start your_local_server_port -p profile2
```

## Related

A introduce article: [Building a HTTP Tunnel with WebSocket and Node.JS](https://medium.com/@embbnux/building-a-http-tunnel-with-websocket-and-node-js-98068b0225d3?source=friends_link&sk=985d90ec9f512928b34ed38b7ddcb378)

## TODO

- [ ] Add tests
- [ ] Support multiple clients based on request path prefix
