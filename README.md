
# websocket - Tiny, cross platform websocket client C library.

修改12345aaaaaaaaaaaaa
  if (error) { 
    printf("\nSocket disconnect with code, error: %i, %s", rws_error_get_code(error), rws_error_get_description(error)); 
  }
  // forget about this socket object, due to next disconnection sequence
  _socket = NULL;
}
// callback trigered on socket connected and handshake done
static void on_socket_connected(rws_socket socket) {
  printf("\nSocket connected");
}
// callback trigered on socket received text
static void on_socket_received_text(rws_socket socket, const char * text, const unsigned int length) {
  printf("\nSocket text: %s", text);
}
..................
// Set socket callbacks
rws_socket_set_on_disconnected(_socket, &on_socket_disconnected); // required
rws_socket_set_on_connected(_socket, &on_socket_connected);
rws_socket_set_on_received_text(_socket, &on_socket_received_text);
```
##### Connect
```c
rws_socket_connect(_socket);
```
##### Send message to websocket
```c
const char * example_text =
  "{\"version\":\"1.0\",\"supportedConnectionTypes\":[\"websocket\"],\"minimumVersion\":\"1.0\",\"channel\":\"/meta/handshake\"}";
rws_socket_send_text(_socket, example_text);
```
##### Disconnect or delete websocket object
Since socket can be connected and we need to send disconnect mesage, not just lazy close, need to wait and than delete object.
Thats why just call ```rws_socket_disconnect_and_release```, its thread safe, and forget about this socket object.
```c
rws_socket_disconnect_and_release(_socket);
_socket = NULL;
```
