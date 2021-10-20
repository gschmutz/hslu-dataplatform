# HSLU Data Science Dataplatform

The platform hosted has been generated using the [platys](http://github.com/trivadispf/platys) with the [platys-modern-data-platform](https://github.com/TrivadisPF/platys-modern-data-platform) stack. 

It is meant to be started using Docker Compose, which we will show further below. 

## Installed Software

The following software is installed on the Ubuntu VM:

* Docker
* Docker Compose
* Platys
* Kafkacat
* Python 3

## Connect to the VM

```bash
ssh -i id_rsa_hslu-dataplatform-1 guidoschmutz@86.119.35.156
```

# Prepare the environment (you only have to do that once!)

After your first login, you might want to change the shell to the bash shell, using the follwoing command

```bash
sudo chsh -s /bin/bash $USER
```

Now let's add your user to the `docker` group

```bash
sudo usermod -aG docker $USER
```

In your home directory, clone the `hslu-dataplatform` project using `git`

```bash
cd
git clone https://github.com/gschmutz/hslu-dataplatform.git
```

Add the IP addresses to `.bash_profile` and add an entry to `/etc/hosts`:

```
export NETWORK_NAME=ens3
export PUBLIC_IP=$(curl ipinfo.io/ip)
export DOCKER_HOST_IP=$(ip addr show ${NETWORK_NAME} | grep "inet\b" | awk '{print $2}' | cut -d/ -f1)

echo "$DOCKER_HOST_IP     dataplatform" | sudo tee -a /etc/hosts

# Prepare Environment Variables into .bash_profile file
printf "export PUBLIC_IP=$PUBLIC_IP\n" >> /home/$USER/.bash_profile
printf "export DOCKER_HOST_IP=$DOCKER_HOST_IP\n" >> /home/$USER/.bash_profile
printf "export DATAPLATFORM_HOME=$PWD/hslu-dataplatform\n" >> /home/$USER/.bash_profile
printf "\n" >> /home/$USER/.bash_profile
sudo chown ${USER}:${USER} /home/$USER/.bash_profile
```

Now exit the VM and reconnect to make the settings active.

## Starting the platform

In order to start the data platform, you have to run `docker-compose up` from the data platform home folder, which is `hslu-dataplatform`:

```bash
cd $DATAPLATFORM_HOME
docker-compose up -d
```

As all docker images are already pre-loaded, you should immediately see a result similar to the one shown below:

```
guidoschmutz@hslu-dataplatform-1:~/hslu-dataplatform$ docker-compose up -d
Creating network "hslu-platform" with the default driver
Creating zookeeper-1       ... done
Creating wetty             ... done
Creating kafka-connect-2   ... done
Creating cloudbeaver       ... done
Creating kafka-connect-1   ... done
Creating adminer           ... done
Creating kcat              ... done
Creating kafka-connect-ui  ... done
Creating streamsets-1      ... done
Creating ksqldb-server-1   ... done
Creating kafka-ui          ... done
Creating mqtt-ui           ... done
Creating postgresql        ... done
Creating mosquitto-1       ... done
Creating markdown-renderer ... done
Creating markdown-viewer   ... done
Creating schema-registry-1 ... done
Creating ksqldb-cli        ... done
Creating kafka-2           ... done
Creating kafka-1           ... done
Creating kafka-3            ... done
Creating akhq               ... done
Creating cmak               ... done
Creating schema-registry-ui ... done
```

Now you can access the home page of the platform. It is hosted behind the public IP. Execute the following command and copy the output to a browser window.

```bash
echo "http://${PUBLIC_IP}" 
```


