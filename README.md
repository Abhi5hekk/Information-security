# Information-security Project Report

### Team Member
Abhishek Kumar(16074002)

# Oblivious Transfer
### Introduction

An oblivious transfer (OT) protocol is a type of protocol in which a sender transfers one of potentially many pieces of information to a receiver, but remains oblivious as to what piece (if any) has been transferred. The receiver is also oblivious of the non accepted messages transmitted by the sender.<br/>
Oblivious Transfer is a fundamental building block for more complex cryptographic protocols such as secure multiparty computation protocols.


## Rabin's oblivious transfer protocol

In Rabin's oblivious transfer protocol, the sender generates an RSA public modulus N=pq where p and q are large prime numbers, and an exponent e relatively prime to λ(N) = (p − 1)(q − 1). The sender encrypts the message m as m^e mod N.<br/>

* The sender sends N, e, and m^e mod N to the receiver.
* The receiver picks a random x modulo N and sends x^2 mod N to the sender. Note that gcd(x,N) = 1 with overwhelming probability, which ensures that there are 4 square roots of x^2 mod N.
* The sender finds a square root y of x^2 mod N and sends y to the receiver.  <br/><br/>


If the receiver finds y is neither x nor −x modulo N, the receiver will be able to factor N and therefore decrypt m^e to recover m. However, if y is x or −x mod N, the receiver will have no information about m beyond the encryption of it. Since every quadratic residue modulo N has four square roots, the probability that the receiver learns m is 1/2.

## 1–2 oblivious transfer

In a 1–2 oblivious transfer protocol, Alice the sender has two messages m_0 and m_1, and Bob the receiver has a bit b, and the receiver wishes to receive m_b, without the sender learning b, while the sender wants to ensure that the receiver receives only one of the two messages. 

* Alice has two messages, m_0, m_1, and wants to send exactly one of them to Bob. Bob does not want Alice to know which one he receives.
* Alice generates an RSA key pair, comprising the modulus N, the public exponent e and the private exponent d
* She also generates two random values, x_0, x_1 and sends them to Bob along with her public modulus and exponent.
* Bob picks b to be either 0 or 1, and selects either the first or second x_b.
* He generates a random value k and blinds x_b by computing v = (x_b+k^e) mod N, which he sends to Alice.
* Alice doesn't know (and hopefully cannot determine) which of x_0 and x_1 Bob chose. She applies both of her random values and comes up with two possible values for k: k_0 = (v-x_0)^d mod N and k_1 = (v-x_1)^d mod N. One of these will be equal to k and can be correctly decrypted by Bob (but not Alice), while the other will produce a meaningless random value that does not reveal any information about k.
* She combines the two secret messages with each of the possible keys, m'_0 = m_0+k_0 and m'_1 = m_1+k_1, and sends them both to Bob.
* Bob knows which of the two messages can be unblinded with k, so he is able to compute exactly one of the messages m_b = m'_b-k
