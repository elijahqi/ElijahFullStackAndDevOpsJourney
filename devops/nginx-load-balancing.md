# Nginx-Config-loadBalancing

This is a simple nginx config example for load balancing made by [me](https://github.com/Astr0-G), which aims to split traffic to different servers. Nginx is from RU!!!

## Showcase demo

[demo](https://teste.splinterforge.xyz/)
the demo server will rotate you to the
[site](http://52.221.237.34/) or other site I hosted by refresh the page.

## Installation

install [nginx](https://www.nginx.com/).

## Config Usage

Polling: The polling method is the default method of Nginx load. As the name suggests, all requests are assigned to different services in chronological order. If the service is down, it can be automatically eliminated. After the configuration is as follows, the 8.8.0.1 service, 8.8.0.2 service and 8.8.0.3 service are rotated.

```python
upstream  polling {

       server    8.8.0.1;

       server    8.8.0.2;

       server    8.8.0.3;

      }
...

server{

    ...

    location / {
        proxy_pass http://polling;
    }

...

}
```

Weights: Specify the weight ratio of each service. The weight is proportional to the access ratio. It is usually used when the performance of the backend service machine is not uniform. The weight of the good performance is allocated to maximize the performance of the server. After the following configuration, the access ratio of the 8.8.0.2 service will be 8.8.0.1 Serves twice as much.

```python
upstream  weights {

       server    8.8.0.1 weight=1;

       server    8.8.0.2 weight=2;

       server    8.8.0.3 weight=3;

      }
...

server{

    ...

    location / {
        proxy_pass http://weights;
    }

...

}
```

Iphash: Each request is allocated according to the hash result of the access ip. After such processing, each visitor accesses a backend service fixedly, as follows (ip_hash can be used in conjunction with weight).

```python
upstream  iphash {
       ip_hash;

       server    8.8.0.1 weight=1;

       server    8.8.0.2 weight=2;

       server    8.8.0.3 weight=3;

      }
...

server {

    ...

    location / {
        proxy_pass http://iphash;
    }

...

}
```

Least connection: Distribute requests to the service with the fewest connections.(least_conn can be used in conjunction with weight).

```python
upstream  least_conn {
       least_conn;

       server    8.8.0.1 weight=1;

       server    8.8.0.2 weight=2;

       server    8.8.0.3 weight=3;

      }
...

server {

    ...

    location / {
        proxy_pass http://least_conn;
    }

...

}
```

Fair: Requests are distributed according to the response time of the backend server, and those with short response times are given priority.

```python
upstream  fair {
       fair;

       server    8.8.0.1 weight=1;

       server    8.8.0.2 weight=2;

       server    8.8.0.3 weight=3;

      }
...

server {

    ...

    location / {
        proxy_pass http://fair;
    }

...

}
```

## Contributing

Pull requests are welcome. For major changes, please open an issue first
to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License

[MIT](https://choosealicense.com/licenses/mit/)
