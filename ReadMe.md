---
title: Safe and Simple Mail Protocol (SSMP)
---
# Safe and Simple Mail Protocol (SSMP)

Respository: [https://github.com/krolaw/ssmp](https://github.com/krolaw/ssmp)

Very early beginnings.

<noscript>
<p><em>Note:</em> <strong style="color:red">Javascript has been disabled</strong>, view this page properly here: https://krolaw.github.io/ssmp/ReadMe</p>
</noscript>

<script src="https://krolaw.github.io/wsd/wsd.js"></script>

Uses the https protocol

<sequence-diagram data="
title: Alice emails Bob
alias: A->ssmp.a.com
alias: B->ssmp.b.com
Alice->A: eMail\nto: bob@b.com\nbodyHash: <sha256>
A->B: eMail\nto: bob@b.com\nfrom: alice@a.com\ntime: 202105221515\nsignature: <sig>\nbodySig: <bodySig>
B-->A: Opportunity to reject:\nTime too far in past
note right: B: DB lookup secret code for\nalice@a.com in Bob's account
B->A: Request public key
A->B:Returns Key
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
alias: b.com->ssmp.b.com
alias: a.com->ssmp.a.com
A->a.com: Allow bob@b.com
a.com->b.com: bob@b.com->alice@a.com\ncode: abc123\ntime: 202105221515\nsignature: <sig>
b.com->a.com: Bob doesn't know you
a.com-->A: Pending
B->b.com: Allow alice@a.com
b.com->a.com: alice@a.com->bob@b.com\ncode: xyz456\ntime: 202105221515\nsignature: <sig>
a.com->b.com: Requests public key
b.com->a.com: Returns key
note right: a.com: Signs time+secret+\nalice->bob@b.com\nwith public key. Checks\nresult matches signature.
a.com->b.com: bob@b.com->alice@a.com\ncode: abc123
b.com->B: Emails successfully shared
A->a.com: Allow bob@b.com
a.com->A: Emails successfully shared
"></sequence-diagram>

## Notes
Max DNS TXT 256bytes

[Elliptic Curve Digital Signature Algorithm](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm)
[Curve25519](https://en.wikipedia.org/wiki/Curve25519)