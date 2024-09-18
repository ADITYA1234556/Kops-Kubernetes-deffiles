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

FIRST ATTACH THE POD WITH THE NODE THAT IS IN THE SAME ZONE AS OUR VOLUME. NOW WE CAN ACCESS OUR VOLUME BY SPECIFYING IN THE POD TERMPLAT 
TO TAKE THE AWS VOLUME AND MENTIONING THE VOLUME ID
THE AWS VOLUME HAS TO BE TAGGED SPECIFICALLY AS KubernetesCluster=indianspices.theaditya.co.uk KubernetesCluster=CLUSTERNAME OR WE WILL GET PERMISSION DENIED

WE CAN ALSO SEE ANOTHER ERROR THAT AWS STORAGE IS NOT FREE BECAUSETHE EXT4 WILL HAVE A LOSTPLUSFOUND FLDER THAT WILL CAUSE THIS IMPACT. WE CAN EITHER ASK MYSQL TO IGNORE THE LOSTPPLUSFOUNDFOLDER OR WE CAN DELETE THE 
LOSTPLUSFOUNDFOLDER BEFORE MYSQL STARTS       

WAY 1: TO ASK MYSQL TO IGNORE    
args:  - "--ignore-db-dir=lost+found"

WAY 2: TO DELETE THE LOST+FOUND FOLDER BEFORE MYSQL STARTS USING INITCONTAINERS
        - name: busybox
          image: busybox:1.28
          volumeMounts:
            - mountPath: /var/lib/mysql
              name: database-data
	  args: ["rm", "-rf", "/var/lib/msql/lost+found"]