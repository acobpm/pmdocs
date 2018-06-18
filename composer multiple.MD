1. prepare ca file

```
cd cd ~/project/fabric4/share/

awk 'NF {sub(/\r/, ""); printf "%s\\n",$0;}' crypto-config/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt > ~/project/fabric4/composer/org1/ca-org1.txt

awk 'NF {sub(/\r/, ""); printf "%s\\n",$0;}' crypto-config/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt > ~/project/fabric4/composer/org2/ca-org2.txt


awk 'NF {sub(/\r/, ""); printf "%s\\n",$0;}' crypto-config/ordererOrganizations/example.com/orderers/orderer.example.com/tls/ca.crt > ~/project/fabric4/composer/ca-orderer.txt

```

2. prepare network file 
mynetwork.json, change the URL to IP. Paste ca file's content to the places which need it. 

```
{
    "name": "mynetwork",
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
            "url": "grpcs://192.168.1.220:7050",
            "grpcOptions": {
                "ssl-target-name-override": "orderer.example.com"
            },
            "tlsCACerts": {
                "pem": "-----BEGIN CERTIFICATE-----\nMIICNjCCAdygAwIBAgIRAISlDT6llcr5+f1i8SXrBmgwCgYIKoZIzj0EAwIwbDEL\nMAkGA1UEBhMCVVMxEzARBgNVBAgTCkNhbGlmb3JuaWExFjAUBgNVBAcTDVNhbiBG\ncmFuY2lzY28xFDASBgNVBAoTC2V4YW1wbGUuY29tMRowGAYDVQQDExF0bHNjYS5l\neGFtcGxlLmNvbTAeFw0xODA2MTUxMDMwNDVaFw0yODA2MTIxMDMwNDVaMGwxCzAJ\nBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1TYW4gRnJh\nbmNpc2NvMRQwEgYDVQQKEwtleGFtcGxlLmNvbTEaMBgGA1UEAxMRdGxzY2EuZXhh\nbXBsZS5jb20wWTATBgcqhkjOPQIBBggqhkjOPQMBBwNCAAQToG2v284jli0+pJjk\n0AYoPl3Q6vcTiXlpq9uwG3yWy9Gkp30LH5PYlWqqO7x3WQJKtuyXQE9+ZRhLHDfN\nRK/go18wXTAOBgNVHQ8BAf8EBAMCAaYwDwYDVR0lBAgwBgYEVR0lADAPBgNVHRMB\nAf8EBTADAQH/MCkGA1UdDgQiBCC6yDfBNICxAyDY/ZJaigmRYMOVDWBlBCZR/vT/\n2DJlyTAKBggqhkjOPQQDAgNIADBFAiEA/jNO7pv5+8tK3R5hxN+oS3fYxb/2q6ni\nkf1Imx6b/xcCIA849Ae4mZuLSW+ALSX23HnS+liDzUBdxez+tGs6qGBi\n-----END CERTIFICATE-----\n"
            }
        }
    },
    "peers": {
        "peer0.org1.example.com": {
            "url": "grpcs://192.168.1.211:7051",
            "eventUrl": "grpcs://192.168.1.211:7053",
            "grpcOptions": {
                "ssl-target-name-override": "peer0.org1.example.com"
            },
            "tlsCACerts": {
                "pem": "-----BEGIN CERTIFICATE-----\nMIICSDCCAe+gAwIBAgIQXQ8JQzFFyJ6reN0JAgC6MTAKBggqhkjOPQQDAjB2MQsw\nCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNU2FuIEZy\nYW5jaXNjbzEZMBcGA1UEChMQb3JnMS5leGFtcGxlLmNvbTEfMB0GA1UEAxMWdGxz\nY2Eub3JnMS5leGFtcGxlLmNvbTAeFw0xODA2MTUxMDMwNDVaFw0yODA2MTIxMDMw\nNDVaMHYxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQH\nEw1TYW4gRnJhbmNpc2NvMRkwFwYDVQQKExBvcmcxLmV4YW1wbGUuY29tMR8wHQYD\nVQQDExZ0bHNjYS5vcmcxLmV4YW1wbGUuY29tMFkwEwYHKoZIzj0CAQYIKoZIzj0D\nAQcDQgAExPccZsi7DwRW1udq3wR+rwJ41yH8m/6gpUYf8twgOK8A5rTTiFPW1Qg/\nmVcVeC3YQrqhcnUaQFxPHfmbnE1+P6NfMF0wDgYDVR0PAQH/BAQDAgGmMA8GA1Ud\nJQQIMAYGBFUdJQAwDwYDVR0TAQH/BAUwAwEB/zApBgNVHQ4EIgQgWSXsuzdEogl3\nlh+L3cZwdBlXm9fyUr3Q7Q+j1MkGQ60wCgYIKoZIzj0EAwIDRwAwRAIgStPc4VZq\ntK54ZwsT+Xz0PO9Bz0ZHmsGFMdZ00yeADbICICYZqZPUdMQISlJoVkeujZYPdHG+\nOnUgCvSqEyuCmQfE\n-----END CERTIFICATE-----\n"
            }
        },
        "peer1.org1.example.com": {
            "url": "grpcs://192.168.1.212:7051",
            "eventUrl": "grpcs://192.168.1.212:7053",
            "grpcOptions": {
                "ssl-target-name-override": "peer1.org1.example.com"
            },
            "tlsCACerts": {
                "pem": "-----BEGIN CERTIFICATE-----\nMIICSDCCAe+gAwIBAgIQXQ8JQzFFyJ6reN0JAgC6MTAKBggqhkjOPQQDAjB2MQsw\nCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNU2FuIEZy\nYW5jaXNjbzEZMBcGA1UEChMQb3JnMS5leGFtcGxlLmNvbTEfMB0GA1UEAxMWdGxz\nY2Eub3JnMS5leGFtcGxlLmNvbTAeFw0xODA2MTUxMDMwNDVaFw0yODA2MTIxMDMw\nNDVaMHYxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQH\nEw1TYW4gRnJhbmNpc2NvMRkwFwYDVQQKExBvcmcxLmV4YW1wbGUuY29tMR8wHQYD\nVQQDExZ0bHNjYS5vcmcxLmV4YW1wbGUuY29tMFkwEwYHKoZIzj0CAQYIKoZIzj0D\nAQcDQgAExPccZsi7DwRW1udq3wR+rwJ41yH8m/6gpUYf8twgOK8A5rTTiFPW1Qg/\nmVcVeC3YQrqhcnUaQFxPHfmbnE1+P6NfMF0wDgYDVR0PAQH/BAQDAgGmMA8GA1Ud\nJQQIMAYGBFUdJQAwDwYDVR0TAQH/BAUwAwEB/zApBgNVHQ4EIgQgWSXsuzdEogl3\nlh+L3cZwdBlXm9fyUr3Q7Q+j1MkGQ60wCgYIKoZIzj0EAwIDRwAwRAIgStPc4VZq\ntK54ZwsT+Xz0PO9Bz0ZHmsGFMdZ00yeADbICICYZqZPUdMQISlJoVkeujZYPdHG+\nOnUgCvSqEyuCmQfE\n-----END CERTIFICATE-----\n"
            }
        },
        "peer0.org2.example.com": {
            "url": "grpcs://192.168.1.213:7051",
            "eventUrl": "grpcs://192.168.1.213:7053",
            "grpcOptions": {
                "ssl-target-name-override": "peer0.org2.example.com"
            },
            "tlsCACerts": {
                "pem": "-----BEGIN CERTIFICATE-----\nMIICSDCCAe+gAwIBAgIQTlyfmOFS81ZBVMZQ4xTTrTAKBggqhkjOPQQDAjB2MQsw\nCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNU2FuIEZy\nYW5jaXNjbzEZMBcGA1UEChMQb3JnMi5leGFtcGxlLmNvbTEfMB0GA1UEAxMWdGxz\nY2Eub3JnMi5leGFtcGxlLmNvbTAeFw0xODA2MTUxMDMwNDVaFw0yODA2MTIxMDMw\nNDVaMHYxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQH\nEw1TYW4gRnJhbmNpc2NvMRkwFwYDVQQKExBvcmcyLmV4YW1wbGUuY29tMR8wHQYD\nVQQDExZ0bHNjYS5vcmcyLmV4YW1wbGUuY29tMFkwEwYHKoZIzj0CAQYIKoZIzj0D\nAQcDQgAElPOMZ/nyjXQdGbvs3kqc9w+q0bTQFsacwy7NwZM2p44dD7POGhuPuwhX\nma92fpjiyAf6JeZftkOrifJreQufjqNfMF0wDgYDVR0PAQH/BAQDAgGmMA8GA1Ud\nJQQIMAYGBFUdJQAwDwYDVR0TAQH/BAUwAwEB/zApBgNVHQ4EIgQg+ALYoLlhqEby\n/3SV5MbN1b1gee10sh2DTBy45gHw5CIwCgYIKoZIzj0EAwIDRwAwRAIgfQHFyX7Y\n79ZzP7K2GB952z07N4AMNK55QpBZDMyO6MICIAY2vdQMq37ArFF6n3MNxs9DVXD9\nJaNnrc47QgZSxru9\n-----END CERTIFICATE-----\n"
            }
        },
        "peer1.org2.example.com": {
            "url": "grpcs://192.168.1.214:7051",
            "eventUrl": "grpcs://192.168.1.214:7053",
            "grpcOptions": {
                "ssl-target-name-override": "peer1.org2.example.com"
            },
            "tlsCACerts": {
                "pem": "-----BEGIN CERTIFICATE-----\nMIICSDCCAe+gAwIBAgIQTlyfmOFS81ZBVMZQ4xTTrTAKBggqhkjOPQQDAjB2MQsw\nCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNU2FuIEZy\nYW5jaXNjbzEZMBcGA1UEChMQb3JnMi5leGFtcGxlLmNvbTEfMB0GA1UEAxMWdGxz\nY2Eub3JnMi5leGFtcGxlLmNvbTAeFw0xODA2MTUxMDMwNDVaFw0yODA2MTIxMDMw\nNDVaMHYxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQH\nEw1TYW4gRnJhbmNpc2NvMRkwFwYDVQQKExBvcmcyLmV4YW1wbGUuY29tMR8wHQYD\nVQQDExZ0bHNjYS5vcmcyLmV4YW1wbGUuY29tMFkwEwYHKoZIzj0CAQYIKoZIzj0D\nAQcDQgAElPOMZ/nyjXQdGbvs3kqc9w+q0bTQFsacwy7NwZM2p44dD7POGhuPuwhX\nma92fpjiyAf6JeZftkOrifJreQufjqNfMF0wDgYDVR0PAQH/BAQDAgGmMA8GA1Ud\nJQQIMAYGBFUdJQAwDwYDVR0TAQH/BAUwAwEB/zApBgNVHQ4EIgQg+ALYoLlhqEby\n/3SV5MbN1b1gee10sh2DTBy45gHw5CIwCgYIKoZIzj0EAwIDRwAwRAIgfQHFyX7Y\n79ZzP7K2GB952z07N4AMNK55QpBZDMyO6MICIAY2vdQMq37ArFF6n3MNxs9DVXD9\nJaNnrc47QgZSxru9\n-----END CERTIFICATE-----\n"
            }
        }
    },
    "certificateAuthorities": {
        "ca.org1.example.com": {
            "url": "https://192.168.1.211:7054",
            "caName": "ca-org1",
            "httpOptions": {
                "verify": false
            }
        },
        "ca.org2.example.com": {
            "url": "https://192.168.1.213:7054",
            "caName": "ca-org2",
            "httpOptions": {
                "verify": false
            }
        }
    }
}
```

3. paste below information into file my-network-org1.json , after *version* before *channel* line
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

4. paste below information into file my-network-org2.json , after *version* before *channel* line
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

5. prepare admin's information 

in folder ~/project/fabric4/share

Org1 : 
```
export ORG1=crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp

rm  ~/project/fabric4/composer/org1/*_sk

cp -p $ORG1/signcerts/A*.pem ~/project/fabric4/composer/org1

cp -p $ORG1/keystore/*_sk ~/project/fabric4/composer/org1
```

Org2 : 
```
export ORG2=crypto-config/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp

rm  ~/project/fabric4/composer/org2/*_sk

cp -p $ORG2/signcerts/A*.pem ~/project/fabric4/composer/org2

cp -p $ORG2/keystore/*_sk ~/project/fabric4/composer/org2
```

6. install card

in folder ~/project/fabric4/composer/deploy

for org1
```
composer card create -p ~/project/fabric4/composer/mynetwork-org1.json -u PeerAdmin -c ~/project/fabric4/composer/org1/Admin@org1.example.com-cert.pem -k ~/project/fabric4/composer/org1/*_sk -r PeerAdmin -r ChannelAdmin -f PeerAdmin@mynetwork-org1.card
```

for org2

```
composer card create -p ~/project/fabric4/composer/mynetwork-org2.json -u PeerAdmin -c ~/project/fabric4/composer/org2/Admin@org2.example.com-cert.pem -k ~/project/fabric4/composer/org2/*_sk -r PeerAdmin -r ChannelAdmin -f PeerAdmin@mynetwork-org2.card
```

7. import the 2 cards

```
composer card import -f PeerAdmin@mynetwork-org1.card --card PeerAdmin@mynetwork-org1

composer card import -f PeerAdmin@mynetwork-org2.card --card PeerAdmin@mynetwork-org2
```

8. install bna file in org1

```
composer network install -c PeerAdmin@mynetwork-org1 -a promise04.bna
```
(optional) if query in peer0 and peer1, you can find promise04 was installed as chaincode
information as below:

Name: promise04, Version: 0.0.1-deploy.0, Path: /var/folders/85/tmkx_fq92h3dgxt9g7l3gpyr0000gn/T/businessnetwork118518-7641-1ct82ho.sfvj, Id: 0e7e4a1592f107c97900d2c8d3bb285dfa59c418f99dbedcbabc5e86e55a777b

8. install bna file in org2

```
composer network install -c PeerAdmin@mynetwork-org2 -a promise04.bna
```
(optional) if query in peer0 and peer1 in org2, you can find promise04 was installed as chaincode

9. create endorsement file Create an endorsement policy file

create endorsement-policy.json file like below:
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

10.  create paticipant for business network, 

request user Alice in org1

```
composer identity request -c PeerAdmin@mynetwork-org1 -u admin -s adminpw -d alice
```

request user Bob in Org2

```
composer identity request -c PeerAdmin@mynetwork-org2 -u admin -s adminpw -d bob
```

11. start network

network only start in one place

```
composer network start -c PeerAdmin@mynetwork-org1 -n promise04 -V 0.0.1-deploy.0 endorsementPolicyFile=endorsement-policy.json -A alice -C alice/admin-pub.pem -A bob -C bob/admin-pub.pem
```
(optional ) when check in the peers, the chaincode promise04 should have initialed

```
 peer chaincode list -C ch1 --instantiated
```

12. create and import card for Org1:

```
composer card create -p ~/project/fabric4/composer/mynetwork-org1.json -u alice -n promise04 -c alice/admin-pub.pem -k alice/admin-priv.pem

composer card import -f alice@promise04.card

composer network ping -c alice@promise04
```

add user mason

```
composer participant add -c alice@promise04 -d '{"$class":"com.acob.promiseme.Couple","partner":"resource:com.acob.promiseme.Couple#2625", "personId":"Mason","firstName":"Mason","lastName":"Zhao","gender":"M","totalLove":0,"balance":0}'

composer identity issue -c alice@trade-network -f jo.card -u jdoe -a "resource:org.acme.trading.Trader#trader1-org1"

composer card import -f jo.card

composer network ping -c jdoe@trade-network```
