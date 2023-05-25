# Kerberos

## What is Kerberos?

Kerberos is an network authentication protocol which uses tickets to allow specific devices to communicate with specific services. Kerberos uses UDP port 88 by default.

Kerberos consists of:

- Clients
- A Key Distribution Center (KDC)
    - An Authentication Server (AS)
    - A Ticket Granting Server (TGS)

## How does authentication work in Kerberos?

Assuming the following criteria is met:

- The client has a principal entry inside the KDC, or more specifically, the AS (in other words, is preauthenticated to Kerberos)

### 1. The client sends a plaintext request for a ticket to the KDC.

The request consists of:

- A user ID.
- A service ID.
- Optionally the user's IP address.
- Optionally the lifetime of the "Ticket Granting Ticket" (TGT).

### 2. The KDC verifies the user and sends back two messages.

The first message is encrypted using the client secret key, which is the hash of the user's password that also has a salt, which is the user's NetID and Kerberos Realm that the user exists in - this is how an example unhashed client key would look like: `myPassword123admin@EXAMPLE.COM`. The algorithm used to hash the salted password is called "string2Key", which outputs a 56-bit hex DES key. The message contains:

- The TGS ID.
- The TGS Session Key.
- The timestamp.

The second message (Ticket Granting Ticket) is encrypted using the private TGS key that is residing on the KDC - the user cannot decrypt this message. The message contains:

- The user ID.
- The TGS ID.
- The timestamp.
- The user's IP address.
- The lifetime of the TGT.
- The TGS Session Key.

The client decrypts the first message using the client secret key.

### 3. The client sends three messages to the TGS.

The first message (Ticket Granting Ticket) is the unchanged TGT message (the second message from step 2). The second message is a plaintext message consisting of:

- The service ID.
- The requested lifetime of ticket.

The third message (User Authenticator) is encrypted using the TGS Session Key and consists of:

- The user ID.
- The timestamp.

These 3 messages are sent to the Ticket Granting Server.

### 4. The TGS verifies the data and returns two messages to the user.

First, the Ticket Granting Server checks if the service ID provided by the plaintext message exists in the database. If the service exists, then the TGS proceeds to decrypt the TGT using the private TGS key, use the TGS Session Key to decrypt the User Authenticator message and then validate the data.

Once the data is validated, the TGS creates two new messages. The first message consists of the following data:

- Service ID.
- Timestamp.
- Lifetime of ticket.
- A newly generated random Service Session Key.

The message is encrypted using the TGS Session Key that both the client and TGS has.

The second message (Service Ticket) is the same as the "Ticket Granting Ticket", which is encrypted using the specific "Service Secret Key" but also has the newly generated "Service Session Key" within it.

Both messages are sent to the user.

### 5. The client sends two messages to the service.

The user decrypts the first message using the TGS Session Key, generates a new message (User Authenticator) and encrypts it using the Service Session Key provided by the first message. The Service Ticket and User Authenticator are then sent to the service. 

### 6. The service decrypts, validates and sends back an encrypted final authentication response.

The message encrypted by the service session key consists of:

- Service ID.
- Timestamp.


## Terminology

### Principals

A unique identity of a user or a service used for authentication. It is used to assign tickets to it in order to enable access to Kerberos-aware services. A principal name consists of several components seperated by a "/" seperator. This is the format of a principal name: `<primary>/<instance>@<realm>`.

- `<primary>` - If the Principal represents a user in a realm, this would be the username. Alternatively if this is a host, this would be the string "host".
- `<instance>` - This is an optional string that usually is omitted for users, but can be used for additional role management. For hosts however, this would be the hostname of the device.
- `<realm>` - Self-explanatory. This also can be omitted, which will cause Kerberos to fallback to the default realm.

### Realm

It is a group of systems managed by a Kerberos KDC. It is very similar to a domain in Active Directory and are linked to eachother. A Kerberos realm should be (by convention) the same as the name of an Active Directory Domain, but in uppercase letters.

### TGT - Ticket Granting Ticket

The "Ticket Granting Ticket" is an encrypted message sent by the AS upon client request. It is encrypted using the TGS (Ticket Granting Session) key

### TGS - Ticket Granting Server

--

### Service

This can be an application or service, like SSH for example.

### AS - Authentication Server

This is the server inside the KDC which has data about users. It verifies

## Resource links

- https://en.wikipedia.org/wiki/Kerberos_(protocol)
- https://www.youtube.com/watch?v=5N242XcKAsM
- https://web.mit.edu/kerberos/krb5-1.5/krb5-1.5.4/doc/krb5-user/Introduction.html#Introduction
- https://docs.oracle.com/cd/E36784_01/html/E37126/kintro-56.html
- https://www.simplilearn.com/what-is-kerberos-article
- https://www.oreilly.com/library/view/kerberos-the-definitive/0596004036/ch03s02s02.html