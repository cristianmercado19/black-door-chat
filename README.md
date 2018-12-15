# Black Door Chat

## A Real p2p and server-less chat

I have developed a **JS chat** with some interesting features:

* Serverless - No intermediary server required
* Decentralized
* Secure & Encrypt3d
* Fast - realtime !
* (1) Supported in Google Chrome, Mozilla Firefox, and Opera
* Well written - Clean Code + BDD
* Extensible
    - You will be able to develop your own UI
* Free, I mean a single espresso macchiato as a donation is welcomed :)

(1) WebRTC is currently supported by Google Chrome, Mozilla Firefox, and Opera, in both their desktop and Android versions. Microsoft’s Internet Explorer and Apple’s Safari have yet to add support for WebRTC.

I have been playing with [Web RTC](https://webrtc.org/) in the last couple of months.

> WebRTC (Web Real-Time Communications) is a technology which enables Web applications and sites to capture and optionally stream audio and/or video media, as well as to exchange arbitrary data between browsers without requiring an intermediary. The set of standards that comprises WebRTC makes it possible to share data and perform teleconferencing peer-to-peer, without requiring that the user install plug-ins or any other third-party software.

As a part of my research, I have developed this project called **Black Door Chat**.

## Hosted Client

Although no host is required, you could access the angular-client-version in: [http://black-door-chat.smart-bricks.net](http://black-door-chat.smart-bricks.net).

## Video demo

Do you wanna see that in action, check the [video](https://github.com/cristianmercado19/black-door-chat/blob/master/media/BlackDoorChat.webm?raw=true):</br>
https://github.com/cristianmercado19/black-door-chat/blob/master/media/BlackDoorChat.webm?raw=true

## Run now

1. Clone repository `https://github.com/cristianmercado19/black-door-chat-angular-view.git`
2. `yarn install`
3. `yarn start`


## About the concept

### Connecting two remote browsers

When two devices want to speak to each other, we need a way for them to exchange contact information such as their IP addresses. In the same way DNS servers help browsers locate remote machines, we can set up a mutually known server to help potential peers locate each other. In WebRTC this is known as a *signalling server*.

### Signaling

Signaling is the process of coordinating communication. In order for a WebRTC application to set up a 'call', its clients need to exchange information:

* Session control messages used to open or close communication.
* Error messages.
* Media metadata such as codecs and codec settings, bandwidth and media types.
* Key data, used to establish secure connections.
* Network data, such as a host's IP address and port as seen by the outside world.

To avoid redundancy and to maximize compatibility with established technologies, signaling methods and protocols are not specified by WebRTC standards. This approach is outlined by `JSEP`, the JavaScript Session Establishment Protocol.

JSEP requires the exchange between peers of offer and answer: the media metadata mentioned above. Offers and answers are communicated in Session Description Protocol format (SDP), which look like this:

```json
v=0
o=- 7614219274584779017 2 IN IP4 127.0.0.1
s=-
t=0 0
a=group:BUNDLE audio video
a=msid-semantic: WMS
m=audio 1 RTP/SAVPF 111 103 104 0 8 107 106 105 13 126
c=IN IP4 0.0.0.0
a=rtcp:1 IN IP4 0.0.0.0
a=ice-ufrag:W2TGCZw2NZHuwlnf
a=ice-pwd:xdQEccP40E+P0L5qTyzDgfmW
a=extmap:1 urn:ietf:params:rtp-hdrext:ssrc-audio-level
a=mid:audio
a=rtcp-mux
a=crypto:1 AES_CM_128_HMAC_SHA1_80 inline:9c1AHz27dZ9xPI91YNfSlI67/EMkjHHIHORiClQe
a=rtpmap:111 opus/48000/2
…
```

**IMPORTANT:** Depending on the level of abstraction you will find I have called **"First Key"** to the *offer* and **"Second Key"** to the *answer*.

### Setup Signaling

Imagine Alice is trying to call Eve. Here's the full offer/answer mechanism in all its gory detail:

Alice will kick-off the communication:

* Alice creates an RTCPeerConnection object.
* Alice creates an offer (an SDP session description) with the RTCPeerConnection *createOffer()* method.
* Alice calls *setLocalDescription()* with his offer.
* Alice stringifies the offer and uses a signaling mechanism to send it to Eve.

Now, it is the Eve's turn:

* Eve calls *setRemoteDescription()* with Alice's offer, so that her RTCPeerConnection knows about Alice's setup.
* Eve calls *createAnswer()*, and the success callback for this is passed a local session description: Eve's answer.
* Eve sets her answer as the local description by calling *setLocalDescription()*.
* Eve then uses the signaling mechanism to send her stringified answer back to Alice.

Finally:

* Alice sets Eve's answer as the remote session description using *setRemoteDescription()*.

## Repositories

I have created three different projects:

* [MVC](https://github.com/cristianmercado19/black-door-chat-mvc)
* [Service](https://github.com/cristianmercado19/black-door-chat-service)
* [View](https://github.com/cristianmercado19/black-door-chat-angular-view)

The key goal: You can easily implement your **own chat view**.
In order to do that, I have based my development in the clasic *MVC pattern*. I have isolated the logic of the UI in *Controllers* which live in the [MVC project](https://github.com/cristianmercado19/black-door-chat-mvc) while the *Views* are in separated repository. Actually the views implement interfaces defined by the *controller*.

You will be able to take [Angular view](https://github.com/cristianmercado19/black-door-chat-angular-view) as an end to end UI implementation.

### [MVC - Model View Controller](https://github.com/cristianmercado19/black-door-chat-mvc)

I mainly concentrated all my effort and my focus on this project to start the development.
I have driven it using *Behavior-Driven-Development* (BDD) technique. Without any doubt, it is **much easier to understand** the requirements with this strategy due to the fact that they are expressed in terms of **Given-When-Then tests**.

I have allocated all the logic in the **controller**, which is the **main orchestrator** between the *View* and the *Service*. My approach is based in [Passive View pattern by Martin Fowler](https://martinfowler.com/eaaDev/PassiveScreen.html). If you want to see that in more details: https://martinfowler.com/eaaDev/PassiveScreen.html

Finally, you will find **the View is really light**. Actually it is a simple implementation of the interfaces required by the controllers.

As an example, one of the most complex views:

`black-door-chat-mvc\lib\room\view\room.view.interface.ts`

```typescript
export interface RoomView {
  showGoHomeAction(): void;
  hideLeaveTheRoomAction(): void;
  showPing(): void;
  addMessageYouAreIn(): void;
  getMessage(): string;
  cleanInput(): void;
  addMessageFromYou(message: string): void;
  addNewMessage(message: string): void;
  showIsTyping(): void;
  disableSendAction(): void;
  showWarningMessagePartnerDisconnected(): void;

  // events
  onGoHome(): void
  onSendMessage(): void;
  onIsTyping(): void;
  onLeaveTheRoom(): void;
}
```

Continue reading about *MVC* repository: https://github.com/cristianmercado19/black-door-chat-mvc

### [Service](https://github.com/cristianmercado19/black-door-chat-service)

The *Service project* is the implementation of the service's interfaces required by... the controller. As you could see, it is all around the mvc project.

Most of the required services have been implemented by a single class called **ChatService**. However, it is not a rigid approach. The only reason, why I have implemented all of the interfaces in one class is to simplify the *Dependency Injection* (DI).

Continue reading about *Service* repository: https://github.com/cristianmercado19/black-door-chat-service

### [Angular View](https://github.com/cristianmercado19/black-door-chat-angular-view)

Last but not least, this is the *Angular implementation* of the View's interfaces.

I got some pitfalls around *Angular's Zones*. This is because the angular component is called from outside (from the controller - which is in the mvc project)

Because of that, I am using ngZone as the example below:

```js
addNewMessage(message: string): void {
  
  this._ngZone.run(() => {
  
    const msg = new Message(false, message);
    this.messages.push(msg);
  
  });
}
```

On the other hand, I have introduced some *factories* to manage the DI pattern in the modules. It is not a big deal, you could find other approaches (probably better than this).

Continue reading about *Angular View* repository: https://github.com/cristianmercado19/black-door-chat-angular-view
