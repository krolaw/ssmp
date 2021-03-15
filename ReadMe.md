---
title: Safe and Simple Mail Protocol (SSMP)
---
# Safe and Simple Mail Protocol (SSMP)

Very early beginnings.

<noscript>
<em>Note:</em> Javascript has been disabled, view this page here: https://krolaw.github.io/ssmp/ReadMe
</noscript>

<script src="https://krolaw.github.io/wsd/wsd.js"></script>

Uses the https protocol

<sequence-diagram data="
title: Alice emails Bob
alias: A->a.com
alias: B->b.com
Alice->A: eMail\nto: bob@b.com\nbodyHash: <sha256>
A->B: eMail\nto: bob@b.com\nfrom: alice@a.com\ntime: 202105221515\nsignature: <sig>\nbodySig: <bodySig>
note right: B: DB lookup secret key for\nalice@a.com in Bob's account
B->DNS:Looks up a.com TXT for public key
DNS->B:Returns Key
note right: B: Signs time+secret with public key.\nChecks result matches signature.
note right: B: Reads Body - Signs time+secret+\nsha256 with public key
B-->A: Opportunity to reject
A-->Alice: Failed
B->A: Accepts email
A->Alice: Successful
"></sequence-diagram>

It is suggested that for speed (and therefore cost), domain and db lookups can happen concurrently.

<sequence-diagram data="
title: Alice and Bob Share Emails
alias: A->Alice
alias: B->Bob
A->a.com: Allow bob@b.com
a.com->b.com: bob->alice@a.com\ncode = abc123
b.com->a.com: Bob doesn't know you
a.com-->A: Pending
B->b.com: Allow alice@a.com
b.com->a.com: alice->bob@b.com\ncode = xyz456
a.com->b.com: bob->alice@a.com\ncode = abc123
b.com->B: Emails successfully shared
A->a.com: Allow bob@b.com
a.com->A: Emails successfully shared
"></sequence-diagram>

## Notes
Max DNS TXT 256bytes

[Elliptic Curve Digital Signature Algorithm](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm)
[Curve25519](https://en.wikipedia.org/wiki/Curve25519)