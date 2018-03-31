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
composer card delete -n PeerAdmin@byfn-network-org1
composer card delete -n PeerAdmin@byfn-network-org2
composer card delete -n alice@trade-network
composer card delete -n bob@trade-network
composer card delete -n admin@trade-network
composer card delete -n PeerAdmin@fabric-network
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
```
export ORG1=crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp

cp -p $ORG1/signcerts/A*.pem ~/project/setup/composer/org1

cp -p $ORG1/keystore/*_sk ~/project/setup/composer/org1

```
4.2 execute following for ORG2
```
export ORG2=crypto-config/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp

cp -p $ORG2/signcerts/A*.pem ~/project/setup/composer/org2

cp -p $ORG2/keystore/*_sk ~/project/setup/composer/org2

```