- This repo is a **beginner guide to Ansible.**

- It will show you briefly the main concepts and its benefits by example.

- The repo has **git tags** starting with the simplest steps and will become a bit more sophisticated every tag.

- At the end of this tutorial you'll have automated the set up of a **cross-cloud software defined network for containers using Weave net, Weave Scope and Docker.**

- We'll deploy two containers one in **DigitalOcean**, another one in **AWS** that will communicate with each other.

- This tutorial will assume that you have two machines running coreOS on DigitalOcean and AWS. You can create them manually or using something like terraform or docker-machine. We provide the docker-machine-bootstrap script so you can use it and modify it for this purpose.

To follow the use case you'll need to:

```
git clone git@github.com:enxebre/ansible-pragmatic-guide.git
```

The use case will guide you through the different steps

![steps](images/tags.png)
