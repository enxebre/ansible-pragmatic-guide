This tutorial is using **ansible 2.0.2.0**

This tutorial will assume that you have two machines running coreOS on DigitalOcean and AWS. You can create them manually or using something like terraform or docker-machine. We provide the docker-machine-bootstrap script so you can use it and modify it for this purpose.

```bash
# AWS
# --amazonec2-access-key AKI******* \
# --amazonec2-secret-key 8T93C******* \

docker-machine create --driver amazonec2 \
--amazonec2-region "eu-west-1" \
--amazonec2-ssh-user core \
--amazonec2-device-name /dev/xvda \
--amazonec2-ami ami-e3d6ab90 \
aws-ansible-workshop

# DigitalOcean
export DOTOKEN=${DOTOKEN}
docker-machine create --driver digitalocean \
--digitalocean-access-token $DOTOKEN \
--digitalocean-region lon1 \
--digitalocean-image coreos-stable \
--digitalocean-ssh-user core \
do-ansible-workshop
```