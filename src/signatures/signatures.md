# Hash functions and signatures

The idea of a hash function is very simple but powerful.

hash(data)->output

A hash function takes in data of any size and produces an output of a fixed size.

The output of a hash function is ‘random looking’ but it is deterministic because if you provide the same input to the hash, you will get the same output.

Hash functions tend to exhibit particular traits, which you should consider if you are implementing your own hash functions from scratch (although it’s not recommended), one of which is called the “Avalanche effect.”

The Avalanche effect is the idea that changing 1 bit in the input of the hash function should change about half the output bits.

The second trait that a hash function should have is preimage resistance.

I’ll define preimage resistance as below:

Given y, you can’t find any x such that hash(x)==y.

You can find it eventually but that will take 2²⁵⁶ guesses which is not doable with current computers.

Hash functions should also have second preimage resistance. What this means is:

Given x and y such that hash(x)==y, you cannot find x¹ where x¹!=x and hash (x¹)==y.

A hash function should also be collision resistant. What this means is nobody can find any x, z such that x!=z and hash(x)==hash(z).

Again, you can find it theoretically after 2¹²⁸ guesses but practically this would take a huge amount of time with today’s hardware.

Practically speaking, collision resistance is harder to achieve. Some previous hash functions such as sha-1 and md5 have their collision resistance broken.

According to Wikipedia, “In early 2005, Vincent Rijmen and Elisabeth Oswald published an attack on a reduced version of SHA-1–53 out of 80 rounds — which finds collisions with a computational effort of fewer than 280 operations.[39]

In February 2005, an attack by Xiaoyun Wang, Yiqun Lisa Yin, and Hongbo Yu was announced.[5] The attacks can find collisions in the full version of SHA-1, requiring fewer than 269 operations. (A brute-force search would require 280 operations.)”

Hash functions are widely used to create digital signatures in blockchains like bitcoin.

### Digital Signatures and Lamport’s signature scheme

Simply put, digital signatures serve as proof that you’re the author of a message. Here’s how it works: You sign a message using a secret key. Then, you share the signature along with a public key. Anyone receiving this can verify that you indeed authored the message and possess the corresponding private key used for signing.

Here is how this is applied in Bitcoin. When you want to spend Bitcoin, you create a transaction. This transaction includes information about the sender, receiver, and the amount of Bitcoin being transferred. To authorize this transaction, you sign it with your private key.

Once you’ve signed the transaction, it’s broadcast to the Bitcoin network. Every node in the network verifies the transaction by checking the digital signature against the public key associated with the sending address. If the signature is valid, it confirms that the sender indeed owns the Bitcoin being spent and has the right to transfer it.

Lamport signatures are a type of cryptographic signature scheme based on a one-time secure hash function. They were proposed by Leslie Lamport in 1979.

Here is how it works:

1. You have a function that generates a public key and a private key. The private key is generated from random characters, and the public key is the hash of the private key. Below is pseudocode to demonstrate this.

```
generate_keys():
 private_key = Array[2][n]
 for i from 0 to n-1:
 for j from 0 to n-1:
 private_key[i][j] = randomly_generate_bits()
 public_key = hash_each_element_of_private_key(private_key)
 return private_key, public_key

```

2. You then create a function called sign that takes in the message to sign and the private key. You hash the message and represent it in bits. Then you iterate through the message bits, and if the bit in the digest is 0, you select the corresponding bit from the first row of the private key and append it to an array called signature. Otherwise, you select the bit from the second row and append it to the signature array. At the end, you’ll have a signature made up of different parts of your private key.

```
sign(message, private_key):
 hashed_message = hash(message) // Hash the message
 signature = Array[n] // One-dimensional array to hold the signature
 for i from 0 to n-1:
 if hashed_message[i] == 0:
 signature[i] = private_key[0][i] // Use the first row of the private key if the corresponding bit in the hash is 0
 else:
 signature[i] = private_key[1][i] // Use the second row of the private key if the corresponding bit in the hash is 1
 return signature

```

3. Next, you implement a function called verify to check the validity of a signature. This function takes the original message, the signature, and the public key as inputs. Here's how it works:

You first hash the original message using the same hash function used during signing, resulting in a digest.

Then, for each bit in the signature, you reconstruct the original digest by selecting the corresponding bit from the public key. If the bit in the signature is matched with the corresponding bit from the public key’s first row, it’s considered as a 0-bit; otherwise, it’s considered as a 1-bit.

After reconstructing the digest, you compare it with the hashed message. If they match, the signature is deemed valid; otherwise, it’s considered invalid.

```
verify(message, signature, public_key):
    hashed_message = hash(message)
    reconstructed_hash = Array[n]  // One-dimensional array to hold the reconstructed hash
    for i from 0 to n-1:
        if signature[i] == public_key[0][i]:
            reconstructed_hash[i] = 0  // Reconstruct the hash using the first row of the public key if the signature matches
        else if signature[i] == public_key[1][i]:
            reconstructed_hash[i] = 1  // Reconstruct the hash using the second row of the public key if the signature matches
        else:
            return false  // If neither matches, the signature is invalid
    return reconstructed_hash == hashed_message  // Return true if the reconstructed hash matches the hashed message, false otherwise

```

I have written an implementation of this in Rust and you can check it out

[https://github.com/amschel99/lsig](https://github.com/amschel99/lsig)

While the Lamport signature algorithm offers a basic method for generating and verifying digital signatures, its direct application to blockchain systems may be considered somewhat trivial or naive. This approach has some inherent limitations and challenges that need to be addressed for practical implementation in real-world blockchain applications. In a subsequent article, we will delve deeper into these limitations and explore more sophisticated methods for ensuring the security and efficiency of digital signatures on blockchains.
