# RSA encryption

RSA is a popular asymetric cryptographic algorithm created by:
- Ron Rivest
- Adi Shamir
- Leonard Adleman

An RSA encrypted message needs 2 keys in order to encrypt and decrypt the message.

## The encryption and decryption process
### Encryption
- Pick 2 prime numbers as p, q.
- Multiply p with q to get N.
- Calculate ϕ(N)=(p-1)*(q-1)
- Choose e which needs to be:
    - `1 < e < ϕ(N)`
    - coprime with N and ϕ(N)
    
The encryption key is (e,N).

You encrypt the message by:
- Getting a block of `m` where the sum of the chars is not bigger than `N`.
- Using the following formula: `encMsg = m^e % N`
    
### Decryption
- From the encryption, we pick a number `d` that:
    - `d*e % ϕ(N) == 1`

The decryption key is (d, N)

To decrypt the message, we need to:
- Use the following formula: `decMsg = encMsg^d % N`.
- Round the resulting number.

## The security side of things

RSA typically is safe, but only when you know how to use it effectively. RSA can introduce a couple of vulnerabilities in different situations, for example:
- the e value cannot be 1 and MUST be coprime with N and ϕ(N).
- It is important to have a huge N number.
- And other types of attacks!

### e = 1 problem
Suppose we have the following RSA public key set: `<245841236512478852752909734912575581815967630033049838269083, 1> as <N, e>`. We can observe that `e` in this case is 1, which is vastly insecure, why? Let's see the mathematical side of things:

1. We pick 2 prime numbers as `p`, `q`.
2. The product of `p` and `q` is `N`.
3. If we choose `e` = 1, we get the following algorithm when encrypting a message: `E = m % N`.
4. Since `m` is smaller than `N`, that means that `E = m`.

See the problem? We didn't encrypt the message at all! Just because the public exponent (e) is equal 1. In this case, we don't even need to know the private exponent value to decrypt the message, since it wasn't encrypted in the first place!.

### Small N

If the N value is a small value, like for example 26909, it is easy to brute-force the p and q factors using only an algorithm with O(n^2) and find that the factors are 71 * 379.

### Useful links
- https://www.ams.org/notices/199902/boneh.pdf