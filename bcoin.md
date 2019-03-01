# BCoin Setup
`ssh root@mainnet.moneygames.io`

NOTE: using certbot the ssl cert was obtained with the email mattcbow@gmail.com

Payserver Setups include:

    environment:
      - NET=free

    environment:
      - NET=testmet
      - HOST=testnet.moneygames.io
      - APIKEY=hunterkey
      - SSL=true
      - NODE_EXTRA_CA_CERTS=/usr/local/share/ca-certificates/self-signed.crt
      
    environment:
      - NET=main
      - HOST=mainnet.moneygames.io
      - APIKEY=f0c5d427ff4a53701ada1117e4face2a0f7ec8e39d775f9764307ddf67b1ab53
      - SSL=true



How to create self-signed cert:

1: Run this command to generate
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/self-signed.key -out /etc/ssl/certs/self-signed.crt

2: Retrieve self-signed certificate
scp root@testnet.moneygames.io:/etc/ssl/certs/self-signed.crt /Users/mattbowyer/Desktop/self-signed.crt
then move that self-signed cetificate into the git folder of a microservice

3: added these commands to the Dockerfile of that microservice:
COPY self-signed.crt /usr/local/share/ca-certificates
RUN update-ca-certificates

4:Add this environment variable to the Docker-compose.yaml
    environment:
      - SSL=true
      - NODE_EXTRA_CA_CERTS=/usr/local/share/ca-certificates/self-signed.crt
      
5:Run this command on the bcoin node to get it running with ssl

bcoin --network=testnet --http-host=0.0.0.0  --wallet-http-host=0.0.0.0 --api-key=hunterkey --wallet-api-key=hunterkey --ssl=true --wallet-ssl=true --ssl-cert=/etc/ssl/certs/bcoin-test.crt --wallet-ssl-cert=/etc/ssl/certs/bcoin-test.crt --ssl-key=/etc/ssl/private/bcoin-test.key --wallet-ssl-key=/etc/ssl/private/bcoin-test.key


