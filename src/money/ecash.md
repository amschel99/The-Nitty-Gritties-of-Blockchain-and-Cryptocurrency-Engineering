# Ecash

Designed to remove the double spending problem. In traditional Ecash, the bank creates a coin, i.e., a digital representation of a coin with a unique serial number on it. It then gives the coin with the serial number to Alice, and Alice can prove the bank produced the coin using something called digital signatures (we will explore them later). Alice gives the coin to Bob and gets the sandwich, and Bob goes back to the bank, and the bank confirms the serial number has not been spent. The problem with this is the bank has to be online.

## Chaumian Ecash

In chaumian Ecash, Alice generates the secret number
Alice then adds some randomness to the secret number so that the bank cannot see the secret number
The bank verifies on that blinded secret number and there is no way it knows that it came from Alice.

Alice requests for a coin b(SN) where b is the blinding factor.
The bank gives sig(b(SN)) where sig is the bank's signature.
Alice can remove the blinding factor and the coin is sig(SN), SN
Alice gives this to bob, Bob checks if this is a valid signature from the bank. The bank also records the secret number and it runs a list of secret numbers to make sure secret numbers aren't used more than once.

What if Alice gave the same to Charlie? This would be a double spend right? Well the way chaumian ecash works is that actually the bank stores more information and incase a double spend is detected, the bank will know that it was Alice who tried to double spend. Initially if no double spend, the bank wouldn't know but if double spend is detected, the bank will know it was alice and this is kind of a punishment and also a motivator for Alice not to do so.

This is a clever method and it has several pros:
1.Peer to peer 2. Offline double spend detection 3. Privacy

It still suffers from a big problem:

The bank can decide that they don't want to play this game with you.

The real question is how can we have a digital payment system that doesn't rely on a central party like a bank
