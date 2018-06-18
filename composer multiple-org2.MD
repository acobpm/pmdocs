1. prepare connection file , only keep parts of org2

{
    "name": "mynetwork",
    "x-type": "hlfv1",
    "version": "1.0.0",
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
    "channels": {
        "ch1": {
            "orderers": [
                "orderer.example.com"
            ],
            "peers": {
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
        "ca.org2.example.com": {
            "url": "https://192.168.1.213:7054",
            "caName": "ca-org2",
            "httpOptions": {
                "verify": false
            }
        }
    }
}

2. copy files to another server:

```
scp -r ~/project/fabric4/composer/org2 fabric@192.168.1.199:~/project/fabric4/composer/

scp -r ~/project/fabric4/share/ fabric@192.168.1.199:~/project/fabric4

```


3. install card

in folder ~/project/fabric4/composer/org2


for org2

```
composer card create -p ./mynetwork-org2.json -u PeerAdmin -c ./Admin@org2.example.com-cert.pem -k ./*_sk -r PeerAdmin -r ChannelAdmin -f PeerAdmin@mynetwork-org2.card
```

7. import the card

```

composer card import -f PeerAdmin@mynetwork-org2.card --card PeerAdmin@mynetwork-org2
```


8. install bna file in org2

```
composer network install -c PeerAdmin@mynetwork-org2 -a promise04.bna
```
(optional) if query in peer0 and peer1 in org2, you can find promise04 was installed as chaincode



10.  create paticipant for business network, 


request user bob in Org2 (optional) don't use it if you create in main server, copy the existing bob file from main server to here. 

```
composer identity request -c PeerAdmin@mynetwork-org2 -u admin -s adminpw -d bob
```



12. create and import card for Org2:

```
composer card create -p ~/project/fabric4/composer/org2/mynetwork-org2.json -u bob -n promise04 -c bob/admin-pub.pem -k bob/admin-priv.pem

composer card import -f bob@promise04.card

composer network ping -c bob@promise04
```

add user vicki

```
composer participant add -c bob@promise04 -d '{"$class":"com.acob.promiseme.Couple","partner":"resource:com.acob.promiseme.Couple#2626", "partnerNickName":"","personId":"Vicki","firstName":"vicki","lastName":"Wang","gender":"F","totalLove":0,"balance":0}'

composer identity issue -c bob@promise04 -f vicki.card -u vicki -a "resource:com.acob.promiseme.Couple#Vicki"

composer card import -f vicki.card

composer network ping -c vicki@promise04
```
