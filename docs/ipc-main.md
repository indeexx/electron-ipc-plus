
# Module: ipcPlus (main process)

## Methods

### ipcPlusM.option(opts)

  - `opts` object
    - `excludeSelf` boolean - exclude send ipc message to main process when calling `ipcPlusM.sendToAll`.

Ipc option used in `ipcPlusM.sendToAll` and `ipcPlusM.sendToWins`.

### ipcPlusM.sendToAll(message[, ...args, option])

  - `message` string - Ipc message.
  - `...args` ... - Whatever arguments the message needs.
  - `option` object - You can indicate the last argument as an IPC option by `ipcPlusM.option({...})`.

Send `message` with `...args` to all opened window and to main process asynchronously.

**NOTE**

This is a broadcast method, it will not recieve callback.

### ipcPlusM.sendToWins(message[, ...args, option])

  - `message` string - Ipc message.
  - `...args` ... - Whatever arguments the message needs.
  - `option` object - You can indicate the last argument as an IPC option by `ipcPlusM.option({...})`.

Send `message` with `...args` to all opened windows asynchronously. The renderer process can handle it by listening to the message through the `electron.ipcRenderer` or `ipcPlus` module.

**NOTE**

This is a broadcast method, it will not recieve callback.

Example:

**Send IPC message (main process)**

```javascript
const ipcPlusM = require('electron-ipc-plus');

ipcPlusM.sendToWins('foobar:say-hello', 'Hello World!');
```

**Receive IPC message (renderer process)**

```html
<html>
  <body>
    <script>
      const {ipcRenderer} = require('electron');

      ipcRenderer.on('foobar:say-hello', (event, text) => {
        console.log(text);  // Prints "Hello World!"
      });
    </script>
  </body>
</html>
```

### ipcPlusM.sendToMain(message[, ...args, callback, timeout])

 - `message` string - Ipc message.
 - `...args` ... - Whatever arguments the message needs.
 - `callback` function - You can specify a callback function to receive IPC reply at the last or the 2nd last argument.
 - `timeout` number - You can specify a timeout for the callback at the last argument. If no timeout specified, it will be 5000ms.

Returns `number` - If we have callback function, a session ID will returned.

Example:

**Send IPC message (main process)**

```javascript
const ipcPlusM = require('electron-ipc-plus');

ipcPlusM.sendToMain('foobar:say-hello', (err, msg) => {
  if ( err && err.code === 'ETIMEOUT' ) {
    console.error('Timeout for ipc message foobar:say-hello');
    return;
  }

  console.log(`foobar replied: ${msg}`);
});
```

**Receive and Reply IPC message (main process)**

```javascript
const {ipcMain} = require('electron');

ipcMain.on('foobar:say-hello', event => {
  event.reply(null, 'Hi');
});
```

### ipcPlusM.sendToWin(win, message[, ...args, callback, timeout])

 - `win` BrowserWindow
 - `message` string - Ipc message.
 - `...args` ... - Whatever arguments the message needs.
 - `callback` function - You can specify a callback function to receive IPC reply at the last or the 2nd last argument.
 - `timeout` number - You can specify a timeout for the callback at the last argument. If no timeout specified, it will be 5000ms.

Returns `number` - If we have callback function, a session ID will returned.

Example:

**Send IPC message (main process)**

```javascript
const ipcPlusM = require('electron-ipc-plus');

ipcPlusM.sendToMain('foobar:say-hello', (err, msg) => {
  if ( err && err.code === 'ETIMEOUT' ) {
    console.error('Timeout for ipc message foobar:say-hello');
    return;
  }

  console.log(`foobar replied: ${msg}`);
});
```

**Receive and Reply IPC message (renderer process)**

```javascript
const {ipcRenderer} = require('electron');

ipcRenderer.on('foobar:say-hello', event => {
  event.reply(null, 'Hi');
});
```

### ipcPlusM.cancelRequest(sessionId)

  - `sessionId` number - The session ID.

Cancel request sent to main or renderer process via `ipcPlusM.sendToMain` or `ipcPlusM.sendToWin`.

## Properties

### ipcPlusM.debug

Turn on/off the debug information. No use in current version.