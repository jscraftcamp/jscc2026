# Phishing for Passkeys

Passkeys sign cryptographically to authenticate.

Private key is stored on the device or in your OS or password manager.

* Relying Party
  * Creates crypto challenges
  * Stores public keys
  * Verifies login
* Client
  * Forwards messages
  * Gatekeeper
* Authenticator
  * creates and stores keys
  * signs challenges
  * Performs 2nd factor

Can be used you for 2FA in one go.

Client an Authenticator can be the same device, e.g. a Phone. Or cross device via QR code for example.

## How does the phishing works?

Classic: Entering credentials in a phishing page.

But even if you look out for it you can get phished.

Password managers have the advantage of checking the Url. But we tend to copy paste it anyway.

Passkeys cannot be just pasted. The relaying party must state who to sign in for, so the domain. Phishing attempts here are stopped because they have the wrong domain and the user can't do it and override it.

Actual phishing protection: the client tells the authenticator the domain, in case of using fake subdomains, and the Authenticator signs the origin and the real site detects that, that it is the wrong domain. The real relaying party can decide what domains are allowed.

That is one reason why you shouldn't give access to your domain to your users to create subdomains.

Passkeys and passwords can be used at the same time. Passkeys are then usually used for more sensitive operations.

## Authentication messages

Authenticator devices have the advantage that you see the connection.

But QR codes feel like magic.

Client and Authenticator must exchange certain information.

Authenticator must be able to execute user verificator. But the how is intentionally unspecified.

Client to Authentication Protocol (CTAP) is the protocol between client and authenticator.

Application level: What messages need to be exchanged.

Transport level: How are the messages exchanged, encoding, which devices, which methods, e.g. USB NFC, Bluetooth Low Energy. These are all short distance connection. But it is low convenience.

## Reinventing QR initiated hybrid transport

Via the internet, replacing the other connection methods.

An attacker can connect to the tunnel server and go men in the middle.

But we still need one proximity packet.

The QR code is for knowing which bluetooth packet is for them.

Even if the attacker listens the bluetooth, and tries to connect through the tunnel server. But the actual sender needs to sign with a private key that belongs to the public code in the QR code.

Computer generates public/private key an sends public key. It also sends a challenge, and the authenticator has to sign ith with their key.

QR Code also controls tunnel server negotiation, encryption of messages.

### Attack

Attacker generates QR code and let's the authenticator scan that QR Code. Now the attacker controls the authentication process.

They can also change the domain for which to sign into in that QR code. That differs from regular

Victim must not notice a fake website and not notice that the authentication UI is fake. It must not notice that it is forced into QR code. It must not notice that the QR code is fake. For other phishing attempts that are not a QR code you need close proximity.

## When you should care

* If you support passkeys for authentication.
* expect highly motivated and skilled attackers.
* expect attackers to get into BTLE range

## Can Relying parties prohibit the QR attack

Not with CTAP, because the relying party is not involved. It is convenient for the user but bad for phishing attempts.

With WebAuthn the relying party can allow only certain credentials, connected with a specific transport. The returning messages says how it was obtained, so the authenticatorData is check. But unfortunately that is not fully signed, so it is still affected.

Android phones can be used to to act as USB device users.

## Prevention methods

More layers of authentication.
Integrity protection for authentication methods like WebAuthn.

WebAuthn is meant for mass email phishing attempts but not for specialized attacks.

Educate your users. The real QR code overlays over the browser which phishing sites cannot do.

Additional protection: Hand out your own authentication devices.

## Blog post

https://www.inovex.de/de/blog/phishing-for-passkeys-an-analysis-of-webauthn-and-ctap/
