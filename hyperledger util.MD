The startFabric.sh under fabric-tools does more than just start the Fabric - it removes existing Fabric Containers and recreates new Containers from the Docker Images. The impact of this is that you lose all your data and your Business Network from the Fabric. All Business Network Cards except PeerAdmin@hlfv1 are now useless.

If you want to stop and start your Fabric after you have created it, retaining your Business Network and data follow these commands:

Change to the directory where the docker-compose.yml file is (e.g. /home/<user>/fabric-tools/fabric-scripts/hlfv1/composer)
Run docker-compose stop to top the Fabric Containers
Run docker-compose start to restart where you left off.

link aws with ssh client

ssh -i ~/aws/hyperledger.pem ubuntu@ec2-18-221-194-119.us-east-2.compute.amazonaws.com

Start rest server in background

nohup composer-rest-server -c admin@promise03 -n always -w true > rest-server.out 2> rest-server.err < /dev/null &

Next time you connect to server and if you want to kill earlier instance then use ps -A | grep "node" command to get PID.
