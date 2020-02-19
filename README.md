# ceph-rbd
kubernetes ceph storage class

1. create storage provisioner (apply 01-CEPH-RBD-provision.yml)

2. create secret
kubectl create secret generic ceph-XXXX \
    --type="kubernetes.io/rbd" \
    --from-literal=key='AQAZDbddc06nKBAA97TcbID2lN3BFBtltsLe7Q==' \
    --namespace=kube-system
    
3. create auth on ceph storage
ceph --cluster ceph auth get-or-create client.kubernetes mon 'allow r' osd 'allow rwx pool=kubernetes'
ceph --cluster ceph auth get-key client.kubernetes
AQASEbddzKdLDhAAoMjizD9zvZMMCrcdI18DDg==


4. create client secret
kubectl create secret generic ceph-XXXX-kubernetes \
    --type="kubernetes.io/rbd" \
    --from-literal=key='AQASEbddzKdLDhAAoMjizD9zvZMMCrcdI18DDg==' \
    --namespace=kube-system
    
5. create storage class (apply 02-CEPH-RBD-storageclass.yml) 

6. test create PVC (apply 03-CEPH-RBD-tets-create-PVC.yml)

# Troubleshot CEPH
 Overall status:HEALTH_WARN
 POOL_APP_NOT_ENABLED: application not enabled on 1 pool(s)

ceph osd pool application enable kubernetes rbd


