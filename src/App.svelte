<script>
  let msgs = []

  const ws = new WebSocket( 'ws://127.0.0.1:3000' )

  // when connection is established...
  ws.onopen = () => {
    ws.send( 'A new user has joined the chat!' )

    ws.onmessage = async msg => {
      // add message to end of msgs array,
      // re-assign to trigger UI update
      const message = await msg.data.text()
      msgs = msgs.concat([message ])
    }
  }

  const send = function() {
    const username = document.querySelector('#username').value
    const txt = document.querySelector('#chatMessage').value;

    const fullMessage = `${username}: ${txt}`;
    ws.send( fullMessage )
    msgs = msgs.concat([ `${username}: ` + txt ])
  }
</script>
<div>
  <h2>Chat with a new friend!</h2>
  <div style="display: block; text-align: left">
    <label for="username">Enter your username:</label>
    <input type="text" placeholder="Enter a username" id="username"/>
  </div>
</div>
<div style="margin: auto; margin-top: 10px; border: 1px black solid; border-radius: 10px ;width: 400px; height: 400px; display: flex; flex-direction: column; padding: 5px">
  <div style="flex: 1;overflow-y: auto;margin-bottom: 10px;">
    {#each msgs as msg}
      <div>{msg}</div>
    {/each}
  </div>
  <div style="display: flex">
    <input type='text' id="chatMessage" on:change={send} placeholder="Enter your message!" />

  </div>
</div>
