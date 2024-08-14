# Chaumian ecash
In chaumian Ecash, Alice generates the secret number
Alice then adds some randomness to the secret number so that the bank cannot see the secret number
The bank verifies on that blinded secret number and there is no way it knows that it came from Alice.

Alice requests for a coin b(SN) where b is the blinding factor.
The bank gives sig(b(SN)) where sig is the bank's signature.
Alice can remove the blinding factor and the coin is sig(SN), SN 
Alice gives this to bob, Bob checks if this is a valid signature from the bank. The bank also records the secret number and it runs a list of secret numbers to make sure secret numbers aren't used more than once.