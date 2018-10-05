# Linode Automation by Using API and StackScript

## Installation
1. Prepare a server to install Salt
2. On the server, run this command to install salt
```
curl -L https://bootstrap.saltstack.com -o install_salt.sh
sudo sh install_salt.sh -P -M
```
3. Install Salt Cloud
`sudo apt-get install salt-cloud`

## Configuration
4. Config cloud provider profile (Reference Document: [Core Configuration](https://docs.saltstack.com/en/latest/topics/cloud/config.html#linode)):
`cd /etc/salt/cloud.providers.d`
```
sudo echo "my-linode-config:
  apikey: $APIKEY
  password: $LINODE_PASSWORD
  ssh_pubkey: $SSH_PUB
  ssh_key_file: $SSH_KEY_FILE_PATH
  driver: linode" > linode.conf
```
5. Config Linode profile
`cd /etc/salt/cloud.profiles.d/`
```
sudo echo "linode_nanode_1024:
  provider: my-linode-config
  size: Nanode 1GB
  image: Ubuntu 18.04 LTS
  location: Atlanta, GA, USA" > linode_profile
```
You can review the image and size lists provided by Linode
`salt-cloud --list-sizes my-linode-config`
`salt-cloud --list-images my-linode-config`

6. Run Salt command to create Linode instance
`salt-cloud -p linode_nanode_1024 <linode instance name>`
