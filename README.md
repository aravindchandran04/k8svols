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
