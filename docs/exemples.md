<script type="text/javascript">
    var url = 'ws://'+window.location.host+'/.websocket/device/v1/live/watch'
    url = url.replace(/([^:])(\/\/+)/g, '$1/')
    console.log("connect:", url)
    var socket = new ReconnectingWebSocket(url, "watch-stream")
    socket.onopen = function(event) {
      console.log('socket connected')
      socket.send(JSON.stringify({msg: 'subscribe', deviceID: 'jPDu6frYs/sxUM='}))
    }
    socket.onclose = function(event) {
      console.log('socket closed')
    }
    socket.onerror = function(event) {
      console.log('socket error')
    }
    socket.onmessage = function(event) {
      console.log('onmessage')
      //console.log(event.data)
      var image = document.getElementById("frame")
      image.src = "data:image/png;base64," + event.data
    }
    window.setInterval(function() {
      if(socket.readyState === socket.OPEN) {
        socket.send(JSON.stringify({msg: 'ping'}))
      }
    }, 5000)
