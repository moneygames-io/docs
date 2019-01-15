# BCoin Setup
`ssh root@mainnet.moneygames.io`

NOTE: using certbot the ssl cert was obtained with the email mattcbow@gmail.com

Payserver Setups include:

    environment:
      - NET=free

    environment:
      - NET=testnet
      - APIKEY=hunterkey
      - SSL=false
      
    environment:
      - NET=mainnet
      - APIKEY=f0c5d427ff4a53701ada1117e4face2a0f7ec8e39d775f9764307ddf67b1ab53
      - SSL=true

