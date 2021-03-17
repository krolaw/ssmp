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
Alice->A: eMail\nto: bob@b.com
A->B: eMail\nto: bob@b.com\nfrom: alice@a.com\ntime: 202105221515\nsignature: <sig>
B-->A: Opportunity to reject:\nTime too far in past/future
note right: B: DB lookup alice@a.com\nin Bob's account
B->A: Request public key
A->B:Returns Key
note right: B: Signs time+alice@a.com+bob@b.com with \npublic key. Checks result matches signature.
B-->A: Opportunity to reject
A-->Alice: Failed
note right: B: Reads Body
B->A: Accepts email
A->Alice: Successful
"></sequence-diagram>

It is suggested that for speed (and therefore cost), key and db lookups happen concurrently.

<sequence-diagram data="
title: Alice and Bob Share Emails
alias: A->Alice
alias: B->Bob
alias: b.com->ssmp.b.com
alias: a.com->ssmp.a.com
A->a.com: Allow bob@b.com
a.com->b.com: bob@b.com->alice@a.com\ntime: 202105221515\nsignature: <sig>
b.com->a.com: Bob doesn't know you
a.com-->A: Pending
B->b.com: Allow alice@a.com
b.com->a.com: alice@a.com->bob@b.com\ntime: 202105221515\nsignature: <sig>
a.com->b.com: Requests public key
b.com->a.com: Returns key
note right: a.com: Signs time+alice@a.com->bob@b.com\nwith public key. Checks signature.
a.com->b.com: bob@b.com->alice@a.com
b.com->B: Emails successfully shared
A->a.com: Allow bob@b.com
a.com->A: Emails successfully shared
"></sequence-diagram>

<sequence-diagram data="
title: Alice Signs Up To Bob's Mailing List
alias: A->Alice
alias: B->Bob
alias: b.com->ssmp.b.com
alias: a.com->ssmp.a.com
A->a.com: Allow bobsweekly@b.com
a.com->b.com: bobsweekly@b.com->alice@a.com\ntime: 202105221515\nsignature: <sig>
b.com->a.com: Requests public key
a.com->b.com: Returns key
note right: a.com: Signs time+bobsweekly->alice@a.com\nwith public key. Checks signature.
b.com->a.com: alice@a.com->bobsweekly@b.com\nOR OK (no reply)
a.com->A: Signup complete
"></sequence-diagram>

## Notes
Max DNS TXT 256bytes

[Elliptic Curve Digital Signature Algorithm](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm)
[Curve25519](https://en.wikipedia.org/wiki/Curve25519)

Time is YYYYmmddHHMMSS000000 (ns 64bit)