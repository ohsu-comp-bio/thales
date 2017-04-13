### docker-kafka-pixy

#### Summary

A dockerized kafka-pixy.

kafka-pixy is a GO gRPC gateway for Kafka.

It has the following advantages:
* Allows HTTP users to access Kafka topics without having to expose Kafka and Zookeeper ports
* Potentially an [extention point](https://github.com/mailgun/kafka-pixy/blob/master/proxy/proxy.go#L138-L190
) to introduce JWT style token authorization

It has the following disadvantages:
* The python [grpc code](https://github.com/mailgun/kafka-pixy/blob/master/gen/python/grpc_pb2_grpc.py) is not as clean as the [kafka native python interface](https://github.com/dpkp/kafka-python#kafkaconsumer)

#### SETUP
```
# build this gRPC gateway
docker build -t kafka-pixy .

# start a  combined zookeeper/kafka container
docker run  -d --name kafka -it -p 9092:9092 -p 2181:2181   matrixsolutions/kafka:0.10.0.0

# If you have your own kafka/zookeeper, then you will need to alter default.yaml to inform proxy where to find kafka & zookeeper

# run the proxy
docker run  -d --name  kafka-pixy   --link kafka:kafka -p 19091:19091 -p 19092:19092     kafka-pixy
```

#### TEST
```

# create a test topic 'foo'
docker exec -it kafka /opt/kafka/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic foo


# send a plain text message via POST
curl  -v -X POST localhost:19092/topics/foo/messages?sync  -H 'Content-Type:text/plain'    -d 'XXX'

# you should see
*   Trying ::1...
* Connected to localhost (::1) port 19092 (#0)
> POST /topics/foo/messages?sync HTTP/1.1
> Host: localhost:19092
> User-Agent: curl/7.43.0
> Accept: */*
> Content-Type:text/plain
> Content-Length: 3
>
* upload completely sent off: 3 out of 3 bytes
< HTTP/1.1 200 OK
< Content-Type: application/json
< Date: Thu, 13 Apr 2017 18:16:59 GMT
< Content-Length: 35
<
{
  "partition": 0,
  "offset": 1
* Connection #0 to host localhost left intact
}


# retrieve the message
curl -G localhost:19092/topics/foo/messages?group=bar
# you should see
{
  "key": null,
  "value": "WFhY",
  "partition": 0,
  "offset": 1
}

```
