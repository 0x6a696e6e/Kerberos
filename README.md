# Kerberos

**KERBEROS**
Similar to NTLMv2,
 **Kerberos** is a mutual authentication
method that has been the default Active Directory
authentication method since Windows 2000.
The Kerberos
network authentication protocol utilizes a ticketing system to
allow users and computers defined in an AD domain to identify
one another over the network in a secure fashion. The Kerberos
authentication scheme relies on symmetric-key cryptography,
using a trusted third-party authorization process (i.e., mediator)
to help facilitate interactions between two parties on the
network. These encryption keys can be created only by the
client, network service, and the KDC (described in the following
list). This helps mitigate against attackers’ attempts to
eavesdrop and conduct replay attacks using Kerberos protocol
messages, because the only ones who should be able to decrypt
messages are trusted objects in the AD domain. The Kerberos
authentication service is made up of many key elements,
including the following elements reproduced from the Kerberos
authentication overview page at https://docs.axway.com:

1 - •  Key Distribution Center (KDC) A service that is
configured on a domain controller (such as Active
Directory on Windows) that provides two domain-related
services:

2 - •  Authentication Service (AS) Authenticates the
Kerberos client against the user database and grants a
Ticket Granting Ticket (TGT) to the client.

3 - •  Ticket Granting Service (TGS) Acts as the trusted
third party for the Kerberos protocol to validate access
for the client to the requested Kerberos service. Once
validated, the client is issued a service ticket for that
service.

4 - •  Ticket Granting Ticket (TGT) An encrypted
identification ticket used for traffic protection. The TGT
has a variable expiration date and is used to obtain a
service ticket from the TGS. The TGT is encrypted with
the secret key of the TGS and contains the client/TGS
session key, its expiration date, and the IP address of the
client, which protects the client from man-in-the-middle
attacks.

5 - •  Service ticket A ticket that is encrypted with the secret
key of the Kerberos service and contains the client ID,
client network address, validity period, and client/server
session key. A Kerberos client obtains a service ticket from
the TGS after presenting a valid TGT.

6 - •  Kerberos client An application or end user requesting
access to the Kerberos service.

7 - •  Kerberos service A server or an application providing a
service (web, database, file, etc.) that the Kerberos client
would like to access.

![image](https://user-images.githubusercontent.com/79219451/129471659-e11fcc31-6cb8-46b9-9cda-86a57dfd3e32.png)

provides an illustration of the Kerberos
authentication process. It begins with a user authentication
request, in which the client forwards the user ID in cleartext to
the AD server Authentication Service. If the user ID is found in
the NTDS.dit database, the AS generates a secret key using a
hash of the user’s password, which is used to encrypt the TGS
session key. The AS sends the client’s TGT and TGS session key
to the client. The client then attempts to decrypt the session key
using the hash of the user’s password that was used during the
authentication request. If successful, the client sends an
encrypted authentication request message to the TGS for the
Kerberos service the client wishes to access. The TGS decrypts
the message from the client, validates the client’s service
request, and sends another TGT and session key to the client.
The client sends the service ticket a new authenticator message
encrypted with the appropriate client/server session key to the
Kerberos service to be accessed (i.e., resource server). The
resource server validates the authenticator message, decrypts
the session key, and validates the timestamp to ensure that the
session/ticket is still valid. The resource server then sends a
confirmation message encrypted with the client/server session
key back to the Kerberos client. Once the message has been
confirmed, the mutual authentication process is completed and
the resource server will process requests from the Kerberos
client

