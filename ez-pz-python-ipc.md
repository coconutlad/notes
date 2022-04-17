# Communicate over the network easily :)
On the server:
```python
import zmq

context = zmq.Context()
socket = context.socket(zmq.PAIR)
socket.bind("tcp://127.0.0.1:5555")
socket.send(b"hah")
msg = socket.recv()
```

On the client:
```python
import zmq

context = zmq.Context()
socket = context.socket(zmq.PAIR)
socket.connect("tcp://localhost:5555")
msg = socket.recv()
socket.send(b"wut"*10000)
```

You can install it like so: `python -m pip install pyzmq`.

This is just a simple pair connection, ZMQ also has other types of connections you can do.
Check them out here: https://learning-0mq-with-pyzmq.readthedocs.io/en/latest/pyzmq/patterns/patterns.html