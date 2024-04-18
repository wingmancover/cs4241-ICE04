<script>
  let msgs = [];
  let username = '';
  const ws = new WebSocket('ws://127.0.0.1:3000');

  ws.onopen = () => {
    ws.onmessage = async msg => {
      const message = await msg.data.text();
      msgs = [...msgs, message];
    };
  };

  function send() {
    const txt = document.querySelector('input[type="text"]').value;
    const message = `${username}: ${txt} (${new Date().toLocaleTimeString()})`;
    ws.send(message);
    msgs = [...msgs, `me: ${txt} (${new Date().toLocaleTimeString()})`];
    document.querySelector('input[type="text"]').value = ''; // Clear the input after sending
  }
</script>

<label>
  Username:
  <input type="text" bind:value={username} placeholder="Enter your username" />
</label>
<input type="text" on:keydown={(event) => { if(event.key === 'Enter') send(); }} placeholder="Type a message" />
<button on:click={send}>Send</button>

{#each msgs as msg}
  <h3>{msg}</h3>
{/each}
