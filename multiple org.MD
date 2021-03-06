# setup multi org hyperledger
## steps

1. create fabric network

1.1 change directory to fabric-samples
```
cd fabric-samples
```
1.2 Download the platform binaries, including cryptogen :
```
curl -sSL https://goo.gl/6wtTN5 | bash -s 1.1.0
```
1.3 change directory into first-network sample 
```
cd first-network
```
1.4 Execute the following commands in sequence from the first-network directory:
```
./byfn.sh -m generate

./byfn.sh -m up -s couchdb -a
```

1.5 delete any 'old' business network cards that may exist in your wallet from previous Fabric environments. It is safe to ignore any errors that state that the business network cards cannot be found:
```
composer card delete -c PeerAdmin@byfn-network-org1
composer card delete -c PeerAdmin@byfn-network-org2
composer card delete -c alice@trade-network
composer card delete -c bob@trade-network
composer card delete -c admin@trade-network
composer card delete -c PeerAdmin@fabric-network
```
1.6 delete your file system card store in your HOME directory as follows:
```
rm -fr $HOME/.composer 
```
1.7 create setup dir 
```
mkdir -p ~/project/setup/composer/org1
mkdir -p ~/project/setup/composer/org2
```

1.8 create new connection file in ~/project/setup/composer/byfn-network.json
```
gedit ~/project/setup/composer/byfn-network.json
```
paste below information 
```
{
    "name": "byfn-network",
    "x-type": "hlfv1",
    "version": "1.0.0",
    "channels": {
        "mychannel": {
            "orderers": [
                "orderer.example.com"
            ],
            "peers": {
                "peer0.org1.example.com": {
                    "endorsingPeer": true,
                    "chaincodeQuery": true,
                    "eventSource": true
                },
                "peer1.org1.example.com": {
                    "endorsingPeer": true,
                    "chaincodeQuery": true,
                    "eventSource": true
                },
                "peer0.org2.example.com": {
                    "endorsingPeer": true,
                    "chaincodeQuery": true,
                    "eventSource": true
                },
                "peer1.org2.example.com": {
                    "endorsingPeer": true,
                    "chaincodeQuery": true,
                    "eventSource": true
                }
            }
        }
    },
    "organizations": {
        "Org1": {
            "mspid": "Org1MSP",
            "peers": [
                "peer0.org1.example.com",
                "peer1.org1.example.com"
            ],
            "certificateAuthorities": [
                "ca.org1.example.com"
            ]
        },
        "Org2": {
            "mspid": "Org2MSP",
            "peers": [
                "peer0.org2.example.com",
                "peer1.org2.example.com"
            ],
            "certificateAuthorities": [
                "ca.org2.example.com"
            ]
        }
    },
    "orderers": {
        "orderer.example.com": {
            "url": "grpcs://localhost:7050",
            "grpcOptions": {
                "ssl-target-name-override": "orderer.example.com"
            },
            "tlsCACerts": {
                "pem": "INSERT_ORDERER_CA_CERT"
            }
        }
    },
    "peers": {
        "peer0.org1.example.com": {
            "url": "grpcs://localhost:7051",
            "eventUrl": "grpcs://localhost:7053",
            "grpcOptions": {
                "ssl-target-name-override": "peer0.org1.example.com"
            },
            "tlsCACerts": {
                "pem": "INSERT_ORG1_CA_CERT"
            }
        },
        "peer1.org1.example.com": {
            "url": "grpcs://localhost:8051",
            "eventUrl": "grpcs://localhost:8053",
            "grpcOptions": {
                "ssl-target-name-override": "peer1.org1.example.com"
            },
            "tlsCACerts": {
                "pem": "INSERT_ORG1_CA_CERT"
            }
        },
        "peer0.org2.example.com": {
            "url": "grpcs://localhost:9051",
            "eventUrl": "grpcs://localhost:9053",
            "grpcOptions": {
                "ssl-target-name-override": "peer0.org2.example.com"
            },
            "tlsCACerts": {
                "pem": "INSERT_ORG2_CA_CERT"
            }
        },
        "peer1.org2.example.com": {
            "url": "grpcs://localhost:10051",
            "eventUrl": "grpcs://localhost:10053",
            "grpcOptions": {
                "ssl-target-name-override": "peer1.org2.example.com"
            },
            "tlsCACerts": {
                "pem": "INSERT_ORG2_CA_CERT"
            }
        }
    },
    "certificateAuthorities": {
        "ca.org1.example.com": {
            "url": "https://localhost:7054",
            "caName": "ca-org1",
            "httpOptions": {
                "verify": false
            }
        },
        "ca.org2.example.com": {
            "url": "https://localhost:8054",
            "caName": "ca-org2",
            "httpOptions": {
                "verify": false
            }
        }
    }
}
```
a. generate ORG1 CA cert , run below commands :
```
cd ~/fabric-tools/fabric-samples/first-network/

awk 'NF {sub(/\r/, ""); printf "%s\\n",$0;}' crypto-config/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt > ~/project/setup/composer/org1/ca-org1.txt
```
paste all content from the generated file to the place INSERT_ORG1_CA_CERT in file byfn-network.json

b. generate ORG1 CA cert , run below commands :
```
cd ~/fabric-tools/fabric-samples/first-network/ (optional)

awk 'NF {sub(/\r/, ""); printf "%s\\n",$0;}' crypto-config/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt > ~/project/setup/composer/org2/ca-org2.txt
```

c. generate orderer CA cert run comments :
```
cd ~/fabric-tools/fabric-samples/first-network/ (optional)
awk 'NF {sub(/\r/, ""); printf "%s\\n",$0;}' crypto-config/ordererOrganizations/example.com/orderers/orderer.example.com/tls/ca.crt > ~/project/setup/composer/ca-orderer.txt

```

paste all content from the generated file ca-org1.txt to the place INSERT_ORG1_CA_CERT in file byfn-network.json
paste all content from the generated file ca-org2.txt to the place INSERT_ORG2_CA_CERT in file byfn-network.json
paste all content from the generated file ca-orderer.txt to the place INSERT_ORG2_CA_CERTINSERT_ORDERER_CA_CERT in file byfn-network.json

save the byfn-network.json file.

3. Customizing the connection profile for Org1

3.1 create file byfn-network-org1.json

```
cp  ~/project/setup/composer/byfn-network.json ~/project/setup/composer/org1/byfn-network-org1.json
```
paste below information into file byfn-network-org1.json , after *version* before *channel* line
```
    "client": {
        "organization": "Org1",
        "connection": {
            "timeout": {
                "peer": {
                    "endorser": "300",
                    "eventHub": "300",
                    "eventReg": "300"
                },
                "orderer": "300"
            }
        }
    },
```

3.2 create file byfn-network-org2.json

```
cp  ~/project/setup/composer/byfn-network.json ~/project/setup/composer/org2/byfn-network-org2.json
```
paste below information into file byfn-network-org2.json , after *version* before *channel* line
```
    "client": {
        "organization": "Org2",
        "connection": {
            "timeout": {
                "peer": {
                    "endorser": "300",
                    "eventHub": "300",
                    "eventReg": "300"
                },
                "orderer": "300"
            }
        }
    },
```

4. prepare public key and private key for admin

```
cd ~/fabric-tools/fabric-samples/first-network
```

4.1 execute following for ORG1

delete existing sk file first.

```
export ORG1=crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp

rm  ~/project/setup/composer/org1/*_sk

cp -p $ORG1/signcerts/A*.pem ~/project/setup/composer/org1

cp -p $ORG1/keystore/*_sk ~/project/setup/composer/org1

```
4.2 execute following for ORG2
```
export ORG2=crypto-config/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp

rm  ~/project/setup/composer/org2/*_sk

cp -p $ORG2/signcerts/A*.pem ~/project/setup/composer/org2

cp -p $ORG2/keystore/*_sk ~/project/setup/composer/org2

```

5. install business network

5.1 create composer admin card for org1 
```
composer card create -p ~/project/setup/composer/org1/byfn-network-org1.json -u PeerAdmin -c ~/project/setup/composer/org1/Admin@org1.example.com-cert.pem -k ~/project/setup/composer/org1/*_sk -r PeerAdmin -r ChannelAdmin -f PeerAdmin@byfn-network-org1.card
```
5.2 create composer admin card for org2
```
composer card create -p ~/project/setup/composer/org2/byfn-network-org2.json -u PeerAdmin -c ~/project/setup/composer/org2/Admin@org2.example.com-cert.pem -k ~/project/setup/composer/org2/*_sk -r PeerAdmin -r ChannelAdmin -f PeerAdmin@byfn-network-org2.card
```
5.3 import the 2 admin card 
```
composer card import -f PeerAdmin@byfn-network-org1.card --card PeerAdmin@byfn-network-org1
composer card import -f PeerAdmin@byfn-network-org2.card --card PeerAdmin@byfn-network-org2
```
5.4 install trade-network for org1
```
composer network install -c PeerAdmin@byfn-network-org1 -a trade-network.bna
```
5.4 install trade-network for org2
```
composer network install -c PeerAdmin@byfn-network-org2 -a trade-network.bna
```
5.5 create endorsement file
Create an endorsement policy file called ~/project/setup/composer/endorsement-policy.json
```
vi ~/project/setup/composer/endorsement-policy.json
```
copy below part to the file
```
{
    "identities": [
        {
            "role": {
                "name": "member",
                "mspId": "Org1MSP"
            }
        },
        {
            "role": {
                "name": "member",
                "mspId": "Org2MSP"
            }
        }
    ],
    "policy": {
        "2-of": [
            {
                "signed-by": 0
            },
            {
                "signed-by": 1
            }
        ]
    }
}
```
6. create paticipant for business network 
6.1 request user Alice in org1 
```
composer identity request -c PeerAdmin@byfn-network-org1 -u admin -s adminpw -d alice
```
The -u admin and the -s adminpw options to this command correspond to the default user registered with the Hyperledger Fabric CA (Certificate Authority).

The certficates will be placed into a directory called alice in the current working directory. There are three certificate files created, but only two are important. These are admin-pub.pem, the certificate (including the public key), and admin-priv.pem, the private key. Only the admin-pub.pem file is suitable for sharing with other organizations. The admin-priv.pem file must be kept secret as it can be used to sign transactions on behalf of the issuing organization.

6.2 request user Bob in Org2
```
composer identity request -c PeerAdmin@byfn-network-org2 -u admin -s adminpw -d bob

```

7. start network 

the version can be checked in package.json in bna zip file.
```
composer network start -c PeerAdmin@byfn-network-org1 -n trade-network -V 0.2.3-deploy.0 endorsementPolicyFile=~/project/setup/composer/endorsement-policy.json -A alice -C alice/admin-pub.pem -A bob -C bob/admin-pub.pem
```

8. create and import card for Org1:

Add Alice (Admin)
```
composer card create -p ~/project/setup/composer/org1/byfn-network-org1.json -u alice -n trade-network -c alice/admin-pub.pem -k alice/admin-priv.pem

composer card import -f alice@trade-network.card

composer network ping -c alice@trade-network
```

add Joe Doe
```
composer participant add -c alice@trade-network -d '{"$class":"org.acme.trading.Trader","tradeId":"trader1-org1", "firstName":"Jo","lastName":"Doe"}'

composer identity issue -c alice@trade-network -f jo.card -u jdoe -a "resource:org.acme.trading.Trader#trader1-org1"

composer card import -f jo.card

composer network ping -c jdoe@trade-network
```
9. create and import card for Org2:

Add Bob (Admin)
```
composer card create -p  ~/project/setup/composer/org2/byfn-network-org2.json -u bob -n trade-network -c bob/admin-pub.pem -k bob/admin-priv.pem

composer card import -f bob@trade-network.card

composer network ping -c bob@trade-network
```

Add Dave Lowe (paticipant)

```
composer participant add -c bob@trade-network -d '{"$class":"org.acme.trading.Trader","tradeId":"trader2-org2", "firstName":"Dave","lastName":"Lowe"}'

composer identity issue -c bob@trade-network -f dave.card -u dlowe -a "resource:org.acme.trading.Trader#trader2-org2"

composer card import -f dave.card

composer network ping -c dlowe@trade-network

```


