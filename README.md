# django-realtime-miniapp
Steps in project:
1. Install django-channels, redis, django websockets client (pip3 install django-channels, redis, websocket-client)
2.  settings.py setup:
 - change from WSGI to ASGI application and set routing (ASGI_APPLICATION = 'jupiter.routing.application')
 - import channel on top of INSTALLED_APPS
 - add redis settings:
      CHANNEL_LAYERS = {
        'default': {
            'BACKEND': 'channels_redis.core.RedisChannelLayer',
            'CONFIG': {
                "hosts": [('127.0.0.1', 6379)],
              },
          },
      }
3. Define routing.py file
4. Add consumer.py (Consumer is like view in WSGI and have 3 main async function connect, disconnect and recive)
5. Use jupyter notebook and django client websockets to emit events:

import json <br>
import requests <br>
import redis <br>
import websocket <br>
import random,time<br>

ws=websocket.WebSocket()

ws.connect('ws://localhost:8000/ws/polData/')

for i in range(1000):<br>
    time.sleep(3)<br>
    ws.send(json.dumps({'value':random.randint(1,100)}))
  
  
6. Use javascript WebSocket patern to subscribe on consumers events:

    	let socket = new WebSocket("ws://localhost:8000/ws/polData/")
    	socket.onopen = function(e) {
    		alert("Connection established");
    	}
    	socket.onmessage = function(e){
    		console.log(e.data)

    	}
    	socket.onclose = function(e){
    		alert("Connection close")
    	}
