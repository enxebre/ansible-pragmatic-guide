- This repo is a **beginner guide to Ansible.**

- It will show you briefly the main concepts and its benefits by example.

- The repo has **git tags** starting with the simplest steps and will become a bit more sophisticated every tag.

- At the end of this tutorial you'll have automated the set up of a **cross-cloud software defined network for containers using Weave net, Weave Scope and Docker.**

- We'll deploy two containers one in **DigitalOcean**, another one in **AWS** that will communicate with each other.

- This tutorial will assume that you have two machines running coreOS on DigitalOcean and AWS. You can create them manually or using something like terraform or docker-machine. We provide the docker-machine-bootstrap script so you can use it and modify it for this purpose.

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

To follow the use case you'll need to:

```
git clone git@github.com:enxebre/ansible-pragmatic-guide.git
```

The use case will guide you through the different steps

![steps](images/tags.png)
