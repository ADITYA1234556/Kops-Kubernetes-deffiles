COMMANDS 

TO CREATE A VOLUME THAT WILL BE USED BY NODES AND PODS:

aws ec2 create-volume --availability-zone=eu-west-2a --size=3 --volume-type=gp2
"VolumeId": "vol-02ce5a280bf1fb13c",

NOW WE NEED A  NODE IN THE SAME AVAILABILTIY ZONE AS OUR VOLUME, SO WE FIND OUT

kubectl get nodes

kubectl describe node i-0421cf9113692c7df | grep -i eu-west-2
 topology.ebs.csi.aws.com/zone=eu-west-2a

kubectl describe node i-0b87b25924988616a | grep -i eu-west-2
 topology.ebs.csi.aws.com/zone=eu-west-2b

