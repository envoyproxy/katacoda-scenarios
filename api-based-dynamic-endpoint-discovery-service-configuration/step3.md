Now you have to start the upstream cluster. For this we are gonna use one example python application:

```
cd assets/upstream/
virtualenv env --python=python2.7
source env/bin/activate
pip install -r requirements.txt
python server.py -p 8081
```{{execute}}


If you see the logs of Envoy, there are errors fetching the endpoints, because we still not started the eds_server.
