The [LACChain ID Framework](https://publications.iadb.org/en/lacchain-id-framework-set-recommendations-blockchain-based-interoperable-privacy-preserving) establishes guidelines for blockchain-based identity solutions (generally classified as Self-Sovereign Identity (SSI)). It addresses identifiers; verifiable credentials and presentations; digital wallets; on-chain PKDs, DNS, OSCPs, roots of trust, and trust frameworks; regulation; and trust frameworks. 

This repository contains the developer documentation developed by the LACChain of a set of open-source tools for Ethereum-based networks intended to facilitate SSI implementations compliant with the LACChain ID Framework and enable compatibility and interoperability among them. This set of tools are made available to enable people and organizations to convey trust between one another on the internet, preventing malicious entities to control, block, censure, delete, or spoof users' identity, or access information that users do not wish to share.

As an important note, personal information must always be kept off-chain.

# Protocol stack

## 1. Decentralized Identifiers (DID)

A DID is "a new type of identifier" that enables verifiable and decentralized digital identity. A DID identify any subject (e.g., a person, organization, thing, data model, abstract entity, etc.) that the controller of the DID decides it identifies. 
A DID is a URI that associates a DID subject with a DID document allowing trustable interactions associated with that subject, as it contains information about the authentication methods to prove ownership of that DID, endpoints, and other attributes.

The manual for the deployment of the component developed for this layer can be downloaded here [DID Manual](./Manual%20Identidad%20-%20DID%20English.pdf)

### 1.1 Universal Resolver

Each DID is associated with a document in JSON format that contains all the public keys, verification methods and services. 
The way to retrieve each DID Document depends on the method itself, however, following the efforts to achieve greater interoperability, 
LACChain has developed a service that aims to resolve any document (although at the moment it only supports more than 7 methods); 
The service is available at the following URL: https://resolver.lacchain.net, and it is also possible to deploy the same tool within any infrastructure through the [Universal Resolver Repository](https://github.com/lacchain/universal-resolver).

### 1.2 LAC DID Method

LACChain has developed and maintains the [LACChain DID method](https://github.com/lacchain/did-method), which is a fully on-chain DID method based on the ERC-1056 standard. The “lac” DID method allows to maintain a fully on-chain management of DIDs, using smart contracts to store the DIDs and the information associated to them.

There is also implementation of [LACChain DID Method in NodeJS](https://github.com/lacchain/lacchain-did-js). It provides the necessary methods and functions to interact with a DID and resolve a DID Document without the need to directly call the smart contracts.


## 2. Verifiable Credentials (VC)

A verifiable credential is a digital file that contains one or more key-value claims (e.g., birth date, name, qualifications, gender, citizenship, etc.) about an entity (the subject), issued by another entity (the issuer), and is verifiable by any entity (the verifier). 

We are building and maintaining a public library of VCs that is aimed to incorporate VCs designed for and used in real use cases across Latin America and the Caribbean in areas such as education, health, energy, public administration services, and land registry, among others. This library is in the domain https://www.lacchain.net/credentials/library/{type}/{hash}/{version} and the VCs are also stored in the LACChain Github  and in the LACChain IPFS nodes.
Verifiable Presentations also need to be verifiable because the recipient must be able to assume that a legitimate credential holder is consenting to share that presentation with them. The mechanism is exactly the same as for Verifiable Credentials, i.e. a "proof" attribute in the VerifiablePresentation object.

The manual for the deployment of the component developed for this layer can be downloaded here [VC Manual](./Manual%20Identidad%20-%20VC%20English.pdf)

### 2.1 Schemas Repository  

Each type of Verifiable Credential (VC) follows the basic scheme proposed by the W3C that defines the fields and data types. 
Similarly, it is possible to extend the proposed standard through schemes that define the new fields within a specific type of credential.

The schema of a VC is a JSON-LD file that describes the fields that the credential can contain, and the credential must point to that file through the "@context" field. 
Therefore, the JSON-LD file associated with the credential schema must exist in a public place where it can be accessed for later validation. 
Within the identity stack and with the purpose of maintaining a control of credential types, LACChain has created a [LACChain Credential Repository](https://github.com/lacchain/vc-registry) where any entity can register its type of credential along with the associated schema for later publication within the registry that is located at https://id.lacchain.net

### 2.2 Verification Process

The LACChain ID Stack comes with the LACChain Verification Process  that  is  presented  in  this  section,  which allows any verifier entity to accomplish diligent electronic verifications of digital credentials presented to them by holders.

The process of verification consists in six steps:

1. Verification of the digital wallet as an electronic service used by the subject
2. Verification of the validity of the credential
3. Verification of the status of the credential
4. Verification of the issuer
5. Verification of the presenter
6. Verification of the claims
   
At LACChain we have proposed that the entire verification process described above be carried out in an on-chain way, that is, using smart contracts based on [EIP-712](https://eips.ethereum.org/EIPS/eip-712) and [EIP-1812](https://eips.ethereum.org/EIPS/eip-1812) for credential signatures and on-chain claims verification, respectively.
One of the proofs of concept has been developed for the issuance of [LACChain Academy academic credentials](https://github.com/lacchain/academy-vc).

### 2.3 Exchange Protocol

Currently, there are not many solutions for the exchange of credentials, some proposals consist of exchange protocols over the internet (see [DIDComm](https://identity.foundation/didcomm-messaging/spec/)).
At LACChain we have developed an ad-hoc solution for the exchange of Verifiable Credentials, exposing a REST API as an SMTP mail service.

The [LACChain Mailbox](https://github.com/lacchain/id-mailbox) is a secure and private system for the exchange of messages, VCs, and VPs. It is a controlled by a centralized service that allows entities identified using DIDs to send and receive messages that are stored encrypted in a secure database.

The manual for the deployment of the component developed for this layer can be downloaded here [Mailbox Manual](./Manual%20Identidad%20-%20Mailbox%20English.pdf)

## 3. Authentication Protocols

In order to access a digital service, we use an authentication method based on OpenID Connect proposed by KayTrust called [DIDConnect](https://developer.kaytrust.id/Specs/DIDConnect/). 
This mechanism makes use of DIDs to perform the authentication. DID Connect introduces the usage of DID and Verifiable Credentials (VCs), which is a decentralized mechanism that allows the client to verify the identity of the user.
The proposed authentication flows, together with the Diagrams, are described in the [Authentication to Service Repository](https://github.com/lacchain/service-authentication).

### 4. On-chain TLs, PKDs, root of trust, and trust frameworks

The verification process of a VC consists of validating its proofs using cryptographic algorithms, ensuring that it has not been altered in any way. However, within this process nothing guarantees that another entity can issue a similar and fully valid credential. 
This last point is where it is necessary to define a Root-of-Trust mechanism that allows verifying which entities have the "authorization" to issue certain types of credentials, thus avoiding that they may be impersonated.

There are currently different centralized solutions to solve this problem, such as: Trusted List (TL) and Public Key Directories (PKD). 
LACChain has defined a form of [Decentralized Root-of-Trust](https://github.com/lacchain/lacchain-pkd), making use of the same concepts but through Smart Contracts, with which TLs and PKDs can be deployed, and associated with the verification process of a VC.

The manual for the deployment of the component developed for this layer can be downloaded here [VC Manual](./Manual%20Identidad%20-%20PKD%20English.pdf)

## License

Copyright 2021 LACChain

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

