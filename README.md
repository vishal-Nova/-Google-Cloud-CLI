# -Google-Cloud-CLI
This  Repository describes how to move a virtual machine (VM) instance between zones or regions in Google Cloud


******************Move a VM instance between zones or regions***********************

Before you begin :: 

Install or update to the latest version of the Google Cloud CLI.
Set a default region and zone.

1) gcloud compute instances list ( This command will help you to list your vm )  

:::: 
NAME: webmumbai
ZONE: asia-south1-c
MACHINE_TYPE: e2-medium
PREEMPTIBLE:
INTERNAL_IP: 10.160.0.2
EXTERNAL_IP: <<<hide>>>
STATUS: RUNNING



2) gcloud compute instances set-disk-auto-delete webmumbai --zone asia-south1-c  --disk disk-snap --no-auto-delete 

> - (Set the auto-delete state webmumbai of to false to ensure that the disks are not
automatically deleted when the VM is deleted)

::: No change requested; skipping update for [webmumbai].


3) gcloud compute instances describe webmumbai --zone  asia-south1-c
(This will help you to describe your vm that you want to move form 1 zone to another region)



4) gcloud compute disks snapshot disk-snap --snapshot-names mybootsnap --zone  asia-south1-c
>-( Create backups of your data by using persistent disk snapshots. ) 


5) gcloud compute instances delete myinstance --zone asia-south1-c
>-(Delete your VM. Deleting your VM shuts it down cleanly and detach any persistent disks. )

6) gcloud compute disks delete disk-snap  --zone asia-south1-c
>-(Delete your DISK)


7) gcloud compute disks create new-disk --source-snapshot   mybootsnap --zone us-west1-b
>-(Create Disk from the sanpshot)

8) gcloud compute instances create vishalshukla --machine-type  e2-micro --zone us-west1-b --disk name=new-disk,boot=yes,mode=rw 
>-(Create instance from disk that is new-disk)

9) gcloud compute snapshots delete mybootsnap
>-( delete snapshot)
