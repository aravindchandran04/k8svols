# Kubernetes Persistent Volumes Source Code

# Commands to remote into masters and agents

	# Copy private key to master node
	scp /root/.ssh/id_rsa  azureuser@52.165.190.149:/home/azureuser/.ssh
	SSH into master node
	ssh -A -i /root/.ssh/id_rsa  azureuser@52.165.190.149
	# From Master node, SSH to agent node using private key
	ssh -i /home/azureuser/.ssh/id_rsa azureuser@k8s-agent-94D29638-2
	


## task-pv-volume.yml

	kind: PersistentVolume
	apiVersion: v1
	metadata:
	  name: task-pv-volume
	  labels:
	    type: local
	spec:
	  capacity:
	    storage: 10Gi
	  accessModes:
	    - ReadWriteOnce
	  hostPath:
	    path: "/tmp/data"

## task-pv-claim.yml

	kind: PersistentVolumeClaim
	apiVersion: v1
	metadata:
	  name: task-pv-claim
	spec:
	  accessModes:
	    - ReadWriteOnce
	  resources:
	    requests:
	      storage: 3Gi

## task-pv-pod.yml

	kind: Pod
	apiVersion: v1
	metadata:
	  name: task-pv-pod
      labels:
         name: ngnix
         app: ngnixapp
	spec:
	
	  volumes:
	    - name: task-pv-storage
	      persistentVolumeClaim:
	       claimName: task-pv-claim
	
	  containers:
	    - name: task-pv-container
	      image: nginx
	      ports:
	        - containerPort: 80
	          name: "http-server"
	      volumeMounts:
	      - mountPath: "/usr/share/nginx/html"
	        name: task-pv-storage


## Useful Commands

	kubectl create -f http://k8s.io/docs/tasks/configure-pod-container/task-pv-volume.yaml
	kubectl create -f http://k8s.io/docs/tasks/configure-pod-container/task-pv-pod.yaml
	kubectl create -f http://k8s.io/docs/tasks/configure-pod-container/task-pv-claim.yaml
	
	
	kubectl get pv task-pv-volume
	kubectl get pvc task-pv-claim
	kubectl get pod task-pv-pod
	kubectl describe pod task-pv-pod
	kubectl exec -it task-pv-pod  /bin/bash
	apt-get update
	apt-get install curl
	curl localhost

## How to clear the screen on master and agent nodes

	echo -e \\033c


## Expose POD as a SERVICE

	apiVersion: v1
	kind: Service
	metadata:
	  labels:
	    name: nginxservice
	  name: nginxservice
	spec:
	  ports:
	    # The port that this service should serve on.
	    - port: 8888
	      name: http
	      targetPort: 80
	  # Label keys and values that must match in order to receive traffic for this service.
	  selector:
	    app: ngnixapp
	  type: LoadBalancer
