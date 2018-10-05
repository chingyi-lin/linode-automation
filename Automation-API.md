# Linode Automation by Using API and StackScript

# Prerequisite
* Install [jq](https://stedolan.github.io/jq/) to parse API's JSON response.
`brew install jq`

# Workthrough
1. Obtain API key from [Linode Manager](https://cloud.linode.com/profile/tokens)
2. Choose “Add a Personal Access Token”
3. Using shell to test the api
```
TOKEN=<Token>
curl -H "Authorization: Bearer $TOKEN" \
    https://api.linode.com/v4/linode/instances/123
```
4. Review this API Doc: [Create Linode Instance](https://developers.linode.com/api/v4#operation/createLinodeInstance)
5. Review Linode types ([API Doc](https://developers.linode.com/api/v4#operation/getLinodeTypes)): 
`curl https://api.linode.com/v4/linode/types`
6. Review Linode regions([API Doc](https://developers.linode.com/api/v4#tag/Regions)):
`curl https://api.linode.com/v4/regions`
7. Review Linode images([API Doc](https://developers.linode.com/api/v4#operation/getImages)):
`curl https://api.linode.com/v4/images`
8. Create parameters
```
TOKEN=<Token >
TYPE=<ex. g6-nanode-1>
IMAGE=<ex. linode/ubuntu18.04>
REGION<ex. us-east>
LABEL=<ex. apiTesting>
STACKSCRIPT_ID=<ex. 343458>
CADDY_USER=<ex. chingyi>
CADDY_PASSWORD=<ex. TestPassw0rd#>
DOMAIN_NAME=<ex. example.com>
ROOT_PASS=<ex. rootPassw0rd#>
```
9. Run this shell script
```
RESPONSE=$(curl -H "Content-Type: application/json" \
    -H "Authorization: Bearer $TOKEN" \
    -X POST -d '{
      "image": $IMAGE,
      "root_pass": $ROOT_PASS,
      "stackscript_id": $STACKSCRIPT_ID,
      "stackscript_data": {
        "caddy_user": $CADDY_USER,
		  "caddy_password": $CADDY_PASSWORD,
        "domain_name": $DOMAIN_NAME
      },
      "booted": true,
      "label": $LABEL,
      "type": $TYPE,
      "region": $REGION"
    }' \
    https://api.linode.com/v4/linode/instances)
```
10. Get the IP
`IP=$(echo $RESPONSE | jq '.ipv4[0]')`
