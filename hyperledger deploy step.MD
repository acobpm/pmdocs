
# Deploy hyperledger
## steps
0. generate bna file 
```
composer archive create -t dir -a promise02.bna -n .
```
1. add business network from runtime  
```
composer network install -c PeerAdmin@hlfv1 -a promise03.bna
```
2. start network
```
composer network start -c PeerAdmin@hlfv1 -A admin -S adminpw -n promise03 -V 0.0.1 -f promise03admin.card
```
3. import card
```
composer card import -f promise03admin.card
```

4. ping the network. (*only after ping , the network will display in playground*)

```
composer network ping -c admin@promise03
```




