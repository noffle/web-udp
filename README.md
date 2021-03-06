# Web UDP
> Experiment for a web standard for creating and using UDP sockets in the browser.

*(More coming soon!)*

## What?

- a low level browser api granting access to UDP sockets for the purpose of peer-to-peer communications with low overhead

## Why?

WebRTC is very heavy on resources: most modern machines cannot open more than a
dozen connections without slowing their entire system to a crawl. Contrast this
to plain UDP sockets, which a modern machine can easily have many many thousands
of at once at very little resource cost.

WebRTC is a very heavy specification. This means a complex implementation, which
only makes the creation of an implementation available to companies/interests
with a great deal of resources. A simpler implementation allows other vendors
and implementations for other languages to come into the table.

These result in webrtc being ineffective for many interesting and exciting
applications like peer-to-peer apps and real-time games.

Having a simple low level layer, folks can layer non-browser-specific modules on
top. It also means less heavy dependency on browser vendors for support, dev,
features, etc. The community can develop out what it wants. This is *so*
powerful.

(NOTE: mention DHTs: https://github.com/feross/webtorrent/issues/288)

## How?

- XORing data with separate send/recv random nonces to scramble payload, to
  prevent web-udp sites from being attacking non-web-udp services /wo explicitly
  supporting web-udp.
- rate limited sending of packets until a web-udp compatible response is
  received from the endpoint, to prevent browers from DOS'ing endpoints.

## Implementation Vectors

No implementation exists yet, but there are a few different options:

- Local server that acts as a UDP proxy to the browser
- Chrome App: these provide [a UDP API](https://developer.chrome.com/apps/sockets_udp)
  - Google has [decided to nix Chrome Apps](https://blog.chromium.org/2016/08/from-chrome-apps-to-web.html)
- [Beaker Browser](https://github.com/beakerbrowser/beaker): integrate natively
- Fork Chromium and write an implementation

## Concerns & Solutions

> 1. Websites would be able to launch DDoS attacks by coordinating UDP packet
>    floods from browsers.

*Q: What keeps Websockets from being abused in this way?*

Maybe sent packets are rate-limited by the browser until a response is received
from the destination? Denial of service would be infeasible against a
non-responsive machine.

> 2. New security holes would be created as JavaScript running in web pages
>    could craft malicious UDP packets to probe the internals of corporate
>    networks and report back over HTTPS.

Scrambling UDP packets using an XOR key would make it so only web-udp-aware apps
would be able to interpret data the browser sends.

> 3. UDP packets are not encrypted, so any data sent over these packets could be
>    sniffed and read by an attacker, or even modified in transmit. It would be
>    a massive step back for web security to create a new way for browsers to
>    send unencrypted packets.

We could either

1. Build in encryption
2. Leave it to userspace solutions to wrap web-udp in various security layers

I prefer #2, since it reduces the surface area of what web-udp does. WebRTC
tried to add many layers of complexity all baked in together, which has made the
result much more difficult to understand and hack on.

## Other Efforts

- [web-udp-public](https://github.com/Maksims/web-udp-public/) looks promising,
  but is intended as a web standard for server-client UDP, not for peer-to-peer
  applications.

- [netcode.io](https://github.com/RedpointGames/netcode.io-browser), which also
  is designed for server-client programs.

## Collaborate

Are you excited about seeing low-level UDP in the browser? File an issue! Send a
PR! Get in contact with [@noffle](https://twitter.com/noffle)!
