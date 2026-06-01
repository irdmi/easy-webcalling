# easy-webcalling
Call via Webrtc just send one link to your friend, don`t waste your time to offer, asnwer sending


A zero‑cost, peer‑to‑peer video calling web app that works directly in the browser – no server setup, no registration, and no monthly bills. Host it on GitHub Pages and start talking.

## Features

- **Direct P2P video & audio** – media traffic goes straight between browsers (WebRTC).
- **One‑click sharing** – copy a short link and send it to a friend; they join with a single click.
- **Optional password protection** – split the room ID into a public URL prefix and a secret password so only people you trust can connect.
- **End‑to‑end encryption** – audio/video are encrypted using DTLS‑SRTP; nobody in between can listen.
- **Connection transparency** – side panel shows your local IP, remote ICE candidates (IPs and types), and real‑time connection states.
- **Mute controls** – toggle microphone and camera on/off during the call.
- **Responsive design** – works on desktop and mobile browsers.
- **Budget: 0** – uses only free infrastructure (PeerJS public signaling server, GitHub Pages).

##  How It Works

1. **Creating a room** – when you click *Create Call*, the app generates a random room identifier (e.g., `abc123`).  
2. **Link sharing** – the identifier (or its public prefix if password‑protected) is placed in the URL hash (`#abc123`). You copy and send this link.  
3. **Joining** – your friend opens the link; the app reads the hash and joins the same room.  
4. **Signaling** – both browsers connect to the free **PeerJS cloud signaling server** (`0.peerjs.com`). This server **only** relays SDP offers/answers and ICE candidates – it never sees your video/audio.  
5. **Direct connection** – once the peers know about each other, WebRTC establishes a direct, encrypted media channel between the two devices.  
6. **Live call** – video and audio flow peer‑to‑peer, bypassing any central server.

If the **password** option is enabled:
- The full room ID is split into a **6‑character prefix** (goes into the URL) and a **secret suffix** (shown only to the creator).
- The joiner must enter the secret suffix manually. Without it, the signaling server cannot match the peers, and the call cannot be established.

##  Getting Started

### Host your own (free)

1. **Fork or copy this repository** into your GitHub account.
2. Make sure the file is named `index.html` (it already is).
3. Go to repository **Settings → Pages**.
   - Under *Source*, select **Deploy from a branch**.
   - Choose `main` (or your default branch) and the `/ (root)` folder.
   - Click **Save**.
4. After a few minutes your site will be live at `https://your-username.github.io/repository-name/`.
5. Open that URL, allow camera/microphone access, and create a call.

No other files, no build steps, no backend required.

### Local development

Because the app uses ES modules and WebRTC, you must serve it via a local HTTP server (opening the file directly via `file://` will not work).

```bash
# In the project folder, run one of:
python3 -m http.server 8000
# or
npx serve .
```

Then open `http://localhost:8000` in your browser.

##  How to Use

1. **Creator** (the person starting the call):
   - Visit the site.
   - Check *Protect with password* if you want extra security.
   - Click **Create Call**.
   - A link appears. Copy it (📋 button) and send it to your friend.
   - If password was enabled, also share the shown password (e.g., dictate it over the phone).

2. **Joiner** (the friend):
   - Open the link.
   - If prompted, enter the password and click **Join**.
   - Allow camera/microphone access when asked.
   - The call will start automatically – you’ll see both videos.

During the call you can:
- Mute your microphone with **🎤 Mute Mic**.
- Turn off your camera with **📷 Mute Cam**.

The **sidebar** shows your local IP, every remote ICE candidate (IP and type), and the current ICE/connection state – so you always know exactly how the two peers are connected.

## Security & Privacy

| Aspect | What’s exposed |
|--------|----------------|
| **Media content** | End‑to‑end encrypted (DTLS‑SRTP). Neither the signaling server nor anyone intercepting the network can decrypt it. |
| **Signaling server (`0.peerjs.com`)** | Sees your IP, the room ID, and the timing of the connection. It does **not** have access to the media stream. |
| **Room ID** | If you use a password, only a short prefix appears in the URL – the full ID cannot be guessed. Without a password, the full ID is in the link; keep the link private. |
| **Man‑in‑the‑middle risk** | Theoretically the signaling server operator could manipulate the signaling data to intercept the call. In practice, this is extremely unlikely for casual use and would require an active attack. For maximum protection, you would need a fully decentralized signaling network (see Future Improvements). |
| **IP addresses** | Visible to the signaling server and to your peer. This is inherent to any direct P2P communication. |

> **Important:** Never share the link publicly if you are not using the password option.
