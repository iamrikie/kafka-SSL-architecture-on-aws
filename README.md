# kafka-SSL-architecture-on-aws

This repo has all the code related to Kafka cluster setup using Docker and Compose with SSL/TLS/HTTPS

![257483367-fc3abeb4-6599-4f57-8fb1-202a01954e1e](https://github.com/user-attachments/assets/a4ddd7a7-b913-4ce2-9732-5b101890109e)



Take one Kafka server and do the following steps


CA(CERTIFICATE AUTHORITY)
-------------------------
- create ca files
- ca.key(private key)
- ca.crt(public key)
- private key has to be within this boundary so cannot be shared to anyone else, rather like a certificate file. It's okay to distribute the public key.

Keystore and Truststore:
------------------------
Next, we need to create a keystore and a truststore.
Now once you have a certificate Authority, Kafka, keystore, truststore, the very first step is to import the ca.crt into your truststore

1. import ca.crt into truststore,

2. once that is done, using the keystore as an input you need to create a file called Kafka unsigned.crt certificate because this is just
created using keystore. it is not yet signed by the cerificate Authority.

3. so now to sign this from a certificate authority, you need to send this file(Kafka unsigned.crt) to CA, CA in turn will use its (ca.key)private key and (ca.crt)public key along with file(Kafka unsigned.crt) and produce a signed certificate file. Now you have a signed certificate(kafka signed.crt) which is ready to be imported into your Keystore.

4. But before importing into keystore, all you need to do is to import ca.crt into keystore.

5. you can import signed certificate(kafka signed.crt) which you have got from CA into your keystore.

so now by this time you have a keystore and Truststore ready, all you need to do is to bootstrap your Kafka and provide the location for Keystore and truststore.
this defines the whole setup.

All the above steps should be done inside the Kafka broker server only.
