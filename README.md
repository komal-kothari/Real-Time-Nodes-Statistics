Real-Time-Nodes-Statistics
==========================

Real Time Nodes Statistics, aka RTNS, is a software providing real time statistics of remote nodes.

The web client shows CPU/Mem/Net/... statistics, node by node. Compared to other tools, the server is not fetching data on a server, the client subscribe to data evolution, and receive new statistic values when they are available. If the supervised node publish a new value each second, the client will get them immediatly, second after second.

The software is based on:
* an python agent publishing statistics from the supervised nodes
* a redis server
* a python web server, compatible with websocket
* a JS web client

![alt tag](https://raw.githubusercontent.com/Orange-OpenSource/Real-Time-Nodes-Statistics/master/doc/archi.png)

Pre-Install
------------

### Download Real-Time-Nodes-Statistics
On Debian like sytems installation:
```
sudo aptitude install git
git clone https://github.com/Orange-OpenSource/Real-Time-Nodes-Statistics.git
```


Server
------------

### Dependencies

The RTNS server needs 4 dependencies: redis server and 3 python libraries for tornado, tornado-redis and redis

On Debian like sytems installation, installation steps are:
```
cd Real-Time-Nodes-Statistics/server/
git clone https://github.com/leporo/tornado-redis.git
cd tornado-redis
sudo python setup.py install
cd ..

wget https://pypi.python.org/packages/source/r/redis/redis-2.10.1.tar.gz
tar zxf redis-2.10.1.tar.gz
cd redis-2.10.1
sudo python setup.py install
cd ..

sudo apt-get install python-tornado redis-server

```
### Configuring Redis

Redis is the database that will contains temporary stats, but it is only opened on localhost by default.
To open it to the public interface, edit /etc/redis/redis.conf and set
```
bind 0.0.0.0

```
If your client are in a secured network, you could not add the requirepass command. If not, do not forget
to add it to you client and server command line

### Running the server

```
./rtns-server.py &
```

Agents
------------

### Dependencies

The RTNS agent needs two libraries. Redis > 2.8 and PSUtils >= 1.2.1

On Debian like sytems installation, installation steps are:
```
cd Real-Time-Nodes-Statistics/agent/
wget https://pypi.python.org/packages/source/r/redis/redis-2.10.1.tar.gz
tar zxf redis-2.10.1.tar.gz
cd redis-2.10.1
sudo python setup.py install
cd ..
sudo apt-get install python-psutil
```

### Running the agent

```
./rtns-agent-redis.py IPADDRESS_OF_RTNS_SERVER &
```

### Errors messages

#### 'module' object has no attribute 'net_io_counters'

The PSUtils module is too old, install it manually
```
sudo apt-get remove  python-psutil
sudo aptitude install python-dev
wget https://github.com/giampaolo/psutil/archive/master.zip
mv master.zip  psutils.zip
unzip psutils.zip 
cd psutil-master/
sudo python setup.py install

```

Interface
------------
To access the interface go to the link written in the server output. For example http://127.0.0.1:8888

