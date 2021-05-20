
  > gcloud components install COMPONENT_ID
  > gcloud components remove COMPONENT_ID


  > gcloud components update
   gcloud components install kubectl

gcloud auth list
gcloud config list
gcloud config set account `ACCOUNT
gcloud info
glcoud help

gcloud help compute instances create

gcloud compute instances list



gcloud init

gcloud config configurations activate alle-dev
gcloud info --format="get(config.paths.active_config_p
gcloud config configurations createath)"

gcloud config list
gcloud config set


# Active accounts
 gcloud auth list


Credentials saved to file: [/Users/wojciech.przechrzta/.config/gcloud/application_default_credentials.json]
gcloud auth application-default login
gcloud auth application-default print-access-token

gcloud config set compute/region europe-west1
gcloud config set compute/zone europe-west1-b

# FLow
gcloud config configurations activate alle-devs
gcloud auth login
gcloud config set project sc-15334-consul-dev

glist
gcloud compute ssh <instancename>  --internal-ip --zone=ZONE
gcloud beta compute ssh --zone "europe-west1-b" "discovery-0wkb"  --internal-ip --project "sc-15334-consul-dev"
gcloud beta compute ssh --zone "europe-west1-b"   --internal-ip --project "sc-15334-consul-dev" <instance-name>
gcloud compute ssh --zone "europe-west1-b"   --internal-ip --project "sc-15334-consul-dev" <instance-name>

gcloud compute ssh INTERNAL_INSTANCE_NAME \
    --zone=ZONE \
    --internal-ip

    gcloud compute ssh INTERNAL_INSTANCE_NAME \
    --zone=ZONE \
    --internal-ip

gcloud compute project-info describe

Install the gcloud command-line tool and configure it to manage your private keys for you.
Forward your private key to the bastion host instance by enabling agent forwarding in your ssh client.

https://cloud.google.com/compute/docs/tutorials/ssh-with-sk

gcloud compute os-login ssh-keys add \
    --project PROJECT_ID \
    --key-file /home/$USER/.ssh/id_ecdsa_sk.pub


gcloud compute ssh VM_NAME --ssh-key-file=/home/$USER/.ssh/id_ecdsa_sk

# Connecting via serial console
gcloud compute instances add-metadata instance-name --metadata serial-port-enable=TRUE

 gcloud compute instances add-metadata discovery-jllm  --metadata serial-port-enable=TRUE
gcloud compute connect-to-serial-port instance-name

gcloud compute connect-to-serial-port instance-name --port 2
gcloud compute project-info describe


  ────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                                 Components                                                 │
├───────────────┬──────────────────────────────────────────────────────┬──────────────────────────┬──────────┤
│     Status    │                         Name                         │            ID            │   Size   │
├───────────────┼──────────────────────────────────────────────────────┼──────────────────────────┼──────────┤
│ Not Installed │ App Engine Go Extensions                             │ app-engine-go            │  4.8 MiB │
│ Not Installed │ Appctl                                               │ appctl                   │ 18.5 MiB │
│ Not Installed │ Cloud Bigtable Command Line Tool                     │ cbt                      │  7.6 MiB │
│ Not Installed │ Cloud Bigtable Emulator                              │ bigtable                 │  6.6 MiB │
│ Not Installed │ Cloud Datalab Command Line Tool                      │ datalab                  │  < 1 MiB │
│ Not Installed │ Cloud Datastore Emulator                             │ cloud-datastore-emulator │ 18.4 MiB │
│ Not Installed │ Cloud Firestore Emulator                             │ cloud-firestore-emulator │ 41.6 MiB │
│ Not Installed │ Cloud Pub/Sub Emulator                               │ pubsub-emulator          │ 60.4 MiB │
│ Not Installed │ Cloud SQL Proxy                                      │ cloud_sql_proxy          │  7.4 MiB │
│ Not Installed │ Emulator Reverse Proxy                               │ emulator-reverse-proxy   │ 14.5 MiB │
│ Not Installed │ Google Cloud Build Local Builder                     │ cloud-build-local        │  6.2 MiB │
│ Not Installed │ Google Container Registry's Docker credential helper │ docker-credential-gcr    │  2.2 MiB │
│ Not Installed │ Kustomize                                            │ kustomize                │ 22.8 MiB │
│ Not Installed │ Minikube                                             │ minikube                 │ 23.7 MiB │
│ Not Installed │ Nomos CLI                                            │ nomos                    │ 19.8 MiB │
│ Not Installed │ On-Demand Scanning API extraction helper             │ local-extract            │ 10.4 MiB │
│ Not Installed │ Skaffold                                             │ skaffold                 │ 17.0 MiB │
│ Not Installed │ anthos-auth                                          │ anthos-auth              │ 16.3 MiB │
│ Not Installed │ config-connector                                     │ config-connector         │ 43.8 MiB │
│ Not Installed │ gcloud Alpha Commands                                │ alpha                    │  < 1 MiB │
│ Not Installed │ gcloud Beta Commands                                 │ beta                     │  < 1 MiB │
│ Not Installed │ gcloud app Java Extensions                           │ app-engine-java          │ 53.1 MiB │
│ Not Installed │ gcloud app PHP Extensions                            │ app-engine-php           │ 21.9 MiB │
│ Not Installed │ gcloud app Python Extensions                         │ app-engine-python        │  6.1 MiB │
│ Not Installed │ gcloud app Python Extensions (Extra Libraries)       │ app-engine-python-extras │ 27.1 MiB │
│ Not Installed │ kpt                                                  │ kpt                      │ 12.2 MiB │
│ Not Installed │ kubectl                                              │ kubectl                  │  < 1 MiB │
│ Not Installed │ kubectl-oidc                                         │ kubectl-oidc             │ 16.3 MiB │
│ Not Installed │ pkg                                                  │ pkg                      │          │
│ Installed     │ BigQuery Command Line Tool                           │ bq                       │  < 1 MiB │
│ Installed     │ Cloud SDK Core Libraries                             │ core                     │ 17.6 MiB │
│ Installed     │ Cloud Storage Command Line Tool                      │ gsutil                   │  3.9 MiB │
└───────────────┴──────────────────────────────────────────────────────┴──────────────────────────┴──────────┘

1. Connecting via bastion host
https://cloud.google.com/compute/docs/instances/connecting-advanced#bastion_host


2. ssh forwarding
https://cloud.google.com/compute/docs/instances/connecting-advanced#linux-and-macos_2
https://cloud.google.com/compute/docs/instances/connecting-advanced#gcloud

- on local host
eval ssh-agent $SHELL
ssh-add ~/.ssh/consul-dev.pem

- to bastion
ssh -A USERNAME@BASTION_HOST_EXTERNAL_IP
ssh -A consul@BASTION_HOST_EXTERNAL_IP
or

gcloud compute ssh --ssh-flag="-A" BASTION_HOST_INSTANCE_NAME

- to instance
ssh USERNAME@INTERNAL_INSTANCE_IP_ADDRESS
ssh consul@<ip>


gcloud compute project-info describe

gcloud compute project-info add-metadata --metadata-from-file ssh-keys=[LIST_PATH]

gcloud compute project-info add-metadata --metadata-from-file ssh-keys=/Users/wojciech.przechrzta/myDevel/helix/discovery-service-terraform/info/wojciech-key
gcloud compute instances add-metadata [INSTANCE_NAME] --metadata block-project-ssh-keys=FALSE
gcloud compute instances describe [INSTANCE_NAME]

 - key: ssh-keys
    value: wojciech.przechrzta:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC0Pok9t0DmGkp6rPBTV377LnzLSveS1i/VCJxsRjm7qnxPWQSe8s/P2uxkLXRiJsnYTtaX5wqq6vzojxXsNGNXjpj+pkJ19H6bZ+VvkrxSgX7O47rH5XEFM8OKLB/wZ2GmKW9TWSHQXgpnT3jfTnJSj0rgWZ1lscaHuhXLxJ97/u8Yr9F+SA21Ry9GUYWe+eHfLWThgz6PELU1dsjpZ5eIMuijEuk+1j8GImxHx0iRroHtf86koNeSa3hCvbrMUsEHIQQc+K+i7NLE4YEdUUb9Hstz4BQnk70RZuYzcCcxhUrmxHTvLZdh4328mEvn97N7YysODEC7Yf5mBNqIryk9uKoF5HtSrTBT3UgvFdTlKagPXgmz/tw3Qi20kv14W/gi4rpyPU9ghNIku/o7w3aTd0o/1MFQ6APjikO2S0ZaW7xpe0Po0u9N14qGcHQTDQ+V+oDGipgLVgYOu6CvlYhQRfm9n4V/GlJzLJaQ5q5nmyFlPYRfrheUoMOZ5XRdcyE=
      wojciech.przechrzta@polpc04963
