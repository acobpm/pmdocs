
# Run hyperledger
## steps

1. when start from apple macbook, use below comment to use node
```
nvm use --lts
```

10. start fabric 
  a. open a terminal 
  b. cd folder
```
cd ~/fabric-tools/fabric-scripts/hlfv11/composer/
```
  c. run script
'''
docker-compose start 
'''
11. stop fabric 
    a. goto folder on step 10. 
    b. run script
'''
docker-compose stop
'''
101. add business network from runtime  
```
composer-rest-server
```
the busines network card use : admin@promise03 