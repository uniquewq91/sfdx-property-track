# Salesforce App

in case push is not working, use following:
sfdx force:source:deploy -p force-app/main/default -u xxx

force-app/main/default/classes/Schedule_BackupProperty.cls  
You can bypass this error by allowing deployments with Apex jobs in the Deployment Settings page in Setup.

## Implementing OAuth 2.0 JWT Bearer Flow to authenticate to scratch orgs

1. create a Self-Signed SSL certificate and private key
    - a private key is used for signing the JWT bearer token payload
    - certificate as the signing secret
    - <https://devcenter.heroku.com/articles/ssl-certificate-self#generate-ssl-certificate>
    - generate a server.pass.key
    - generate server.key private key using server.pass.key, which contains your private key
    - generate server.csr certificate using server.key private key
    - generate SSL certificate server.cert using server.csr certificate and server.key private key

2. create connected app in DevHub
    - create permset and assign to users

3. must first login to DebHub using sfdx force:auth:jwt:grant \
    --clientid [YOUR_CONSUMER_KEY] \
    --username [YOUR_DEVHUB_LOGIN] \
    --jwtkeyfile ~/.certs/server.key \
    [--setdefaultdevhubusername -a HubOrg] [optional]

4. login to Scratch Org using sfdx force:auth:jwt:grant --clientid [YOUR_CONSUMER_KEY] \
    --username [YOUR_SCRATCHORG_USERNAME] \
    --jwtkeyfile ~/.certs/server.key \
    --instanceurl https://test.salesforce.com
