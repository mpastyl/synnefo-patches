websocket support for vncauthproxy

The folowing changes were made to provide weboscket support for the console.

1)proxy.py
  Reconstruction to add webosocket support. The incoming connection now can be a websocket connection, handled by gevent-websocket.handler at a running instance of gevent.pywsgi server. Recv-sendall methods have been overriden to translate to receive-send methods used by the websocket library. The server is expecting a secure connection, on an ssl-wraped socket.

  Currently, in order to connect to a secure websocket, you first need to make an https connection to the host at the given port just to get the promt so your browser can make an exception for this hostname-port.

2)client.py
  Request_forwarding need one extra parammeter, --console-type wich can be either vnc or wsvnc to indicate normal tcp connection versus websocket connection. This parammeter is send to the control socket via json format and received by proxy.py

3)./kamaki client
  Added one extra parammeter to the console command. Console type is now needed and can be either vnc or wsvnc. Its value is passed via json format during the synnefo-api call.

4)actions.py
  Check console type value and pass it as argument to request_forwarding method of client.py.

5)ui
  /ui/templates/machines_console.html is the html run at the popup window and its modified to redirect to /static/ui/static/snf/extra/noVNC/vnc_auto.html which is the html5 client with websocket support.

  Also, right now, the models.js script, run at console button press, makes the api call using wsvnc value as the default console type.

  To switch back to the old console, models.js needs to send vnc as console_type value and machines_console.html need to go back the way it was.

