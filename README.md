# Pixel Streamer: Unreal + WebRTC

Compared to EpicGame's heavily-designed SDK for Pixel Streaming, PixelStreamer is a lightweight WebRTC library with 0 dependency, containing a frontend component (using WebComponents API), along with a signaling server (using NodeJS).

- peer-stream.js (20KB): https://xosg.github.io/PixelStreamer/peer-stream.js
- signal.js (5KB): https://xosg.github.io/PixelStreamer/signal.js
- WebSocket for NodeJS: https://www.npmjs.com/package/ws
- EpicGame's SDK: https://github.com/EpicGames/UnrealEngine/tree/release/Samples/PixelStreaming/WebServers/SignallingWebServer
- Pixel Streaming Plugin: https://github.com/EpicGames/UnrealEngine/tree/release/Engine/Plugins/Media/PixelStreaming

## Signaling Server

install WebSocket dependency:

```
npm install ws@8.2.3
node signal.js {key}={value}
```

startup options:

| key    | default | usage                    |
| ------ | ------- | ------------------------ |
| player | 88      | browser port             |
| engine | 8888    | unreal engine port       |
| token  | insigma | password appended to URL |
| limit  | 4       | max number of players    |

## Unreal Engine 4

enable the plugin:

```
Plugins > Built-In > Graphics > Pixel Streaming > Enabled
Editor Preferences > Level Editor > Play > Additional Launch Parameters
start myPackagedGame.exe -{key}={value}
```

common startup options:

```
 -ForceRes
 -ResX=1920
 -ResY=1080
 -AudioMixer
 -RenderOffScreen
 -graphicsadapter=0
 -AllowPixelStreamingCommands
 -PixelStreamingEncoderRateControl=VBR
 -PixelStreamingURL="ws://localhost:8888"
```

## Browser

JavaScript:

```
import "peer-stream.js";
const ps = document.createElement("video", { is: "peer-stream" });
ps.setAttribute("signal", "ws://127.0.0.1:88/insigma");
document.body.appendChild(ps);
```

or HTML:

```
<script src="peer-stream.js"></script>
<video is="peer-stream" signal="ws://127.0.0.1:88/insigma"></video>
```

## APIs

lifecycle:

```
ps.addEventListener("open", e => {});
ps.addEventListener("message", e => {});
ps.addEventListener("close", e => {});
```

Mouse, Keyboard, Touch events:

```
ps.registerTouchEvents()
ps.registerKeyboardEvents()
ps.registerFakeMouseEvents()
ps.registerMouseHoverEvents()
ps.registerPointerlockEvents()
```

## Common Commands

```
ps.debug('PLAYER.clients.size')   // show players count
ps.debug('PLAYER.clients.forEach(p=>p.playerId!==playerId&&p.close(1011,"Infinity"));limit=1;')  // kick other players
ps.debug('[...PLAYER.clients].map(x=>x.req.socket.remoteAddress)')  // every player's IP
ps.debug('playerId')  // show my id
ps.pc.getReceivers().find(x=>x.track.kind==='video').transport.iceTransport.getSelectedCandidatePair().remote    // show selected candidate
ps.addEventListener('mouseenter',_=>{ps.focus();ps.requestPointerLock()})    // pointer lock
ps.style.pointerEvents='none'   // read only <video>
```

## Requirement

- Google Chrome 88+
- Unreal Engine 4.27+
- NodeJS 14+
- npm/ws 8.0+

## License

[MIT License](./LICENSE)

Copyright (c) 2021 XOSG
