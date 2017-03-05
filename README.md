# Kubernetes Persistent Volumes Source Code

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
	kubectl exec -it task-pv-pod -- /bin/bash
	apt-get update
	apt-get install curl
	curl localhost

