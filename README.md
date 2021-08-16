# pqc-auth-dns
PQC-aware DNS configuration for mgrillere.com authoritative name server meant as playground

## Architecture
```
           mgrillere.com 
                 |
   _____________________________
  |          |          |       |
falcon    baseline    both    bogus   
```
The upper zone is secured with ECDSA algorithm 13 and it's DS record is published to ensure the right behavior of the chain-of-trust.
The following subzone are secured by algorithms specified as follows:
- Falcon is secured with ZSK and KSK using Falcon-512 (Algorithm 17) and 10248 Bits key
- Baseline is secured with ZSK and KSK using Ed448 (Algorithm 16) and 456 Bits key
- Both is secured with 2 KSK and 2 ZSK, one pair using the Ed448 keys and one pair using Falcon-512 keys
- Bogus is secured using KSK and ZSK using Falcon-512 and 10248 Bits keys BUT the public key of the ZSK has been tweak to be "wrong", i.e not correspond to the private key

## Use Case
The baseline zonne is used to have a reference point in term of traffic and also to have a zone wich is compliant to nowadays supported algorithms.
The falcon zone is a playground zone to test Flacon-512 validation for recursor and also to benchmark traffic created by PQC algorithm.
The both zone is an example of what could a transition zone look like in the future, also good to benchmark the packet size and created traffic.
The bogus zone is a test zone to ensure that the validation does not accept everything and no recursor correctly configured should be able to validate answer coming from this zone.
