Day 54 – Hands-on Tasks 
This section provides a complete practical walkthrough of Kubernetes ConfigMaps and Secrets. Each task includes commands, explanations, and expected outcomes.

# Task 1: Create a ConfigMap from Literals

Create a ConfigMap using command-line key-value pairs.

# Command
kubectl create configmap app-config \
  --from-literal=APP_ENV=production \
  --from-literal=APP_DEBUG=false \
  --from-literal=APP_PORT=8080
  
# Explanation
kubectl create configmap app-config → Creates a ConfigMap named app-config
--from-literal → Adds key-value pairs directly from CLI

# Verify
kubectl get configmap app-config -o yaml

# Expected Result
You should see all three keys under data
Values are stored as plain text

# Task 2: Create a ConfigMap from a File

Store a full configuration file inside a ConfigMap.

# Step 1: Create a file
nano default.conf

Paste:

server {
    listen 80;

    location /health {
        return 200 'healthy';
        add_header Content-Type text/plain;
    }
}
# Step 2: Create ConfigMap
kubectl create configmap nginx-config \
  --from-file=default.conf=default.conf
  
# Explanation
Left side (default.conf) → Key name
Right side → File content

# Verify
kubectl get configmap nginx-config -o yaml

# Expected Result
File content appears inside ConfigMap under data
Stored as multiline string

# Task 3: Use ConfigMap in a Pod

# Part A: Inject as Environment Variables
# YAML
apiVersion: v1
kind: Pod
metadata:
  name: env-demo
spec:
  containers:
  - name: test-container
    image: busybox
    command: ["sh", "-c", "env && sleep 3600"]
    envFrom:
    - configMapRef:
        name: app-config
      
# Explanation
envFrom injects all ConfigMap keys as environment variables
env command prints variables

# Run
kubectl apply -f pod-env.yaml
kubectl logs env-demo

# Expected Output
APP_ENV=production
APP_DEBUG=false
APP_PORT=8080
- Part B: Mount as Volume (File-based config)
  
# YAML
apiVersion: v1
kind: Pod
metadata:
  name: nginx-demo
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - name: config-vol
      mountPath: /etc/nginx/conf.d
  volumes:
  - name: config-vol
    configMap:
      name: nginx-config
    
# Explanation
ConfigMap is mounted as files inside the container
Each key becomes a file

# Test
kubectl exec nginx-demo -- curl -s http://localhost/health

# Expected Output
healthy

#  Task 4: Create a Secret

Store sensitive data securely.

# Command
kubectl create secret generic db-credentials \
  --from-literal=DB_USER=admin \
  --from-literal=DB_PASSWORD=s3cureP@ssw0rd
  
# Explanation
Secrets store sensitive values
Data is base64 encoded (not encrypted)

#  Verify
kubectl get secret db-credentials -o yaml

#  Decode Secret
kubectl get secret db-credentials \
  -o jsonpath='{.data.DB_PASSWORD}' | base64 --decode
  
#  Expected Output
s3cureP@ssw0rd

#  Task 5: Use Secret in a Pod

Consume Secret as environment variable and file.

# YAML
apiVersion: v1
kind: Pod
metadata:
  name: secret-demo
spec:
  containers:
  - name: test-container
    image: busybox
    command: ["sh", "-c", "env && cat /etc/db-credentials/DB_PASSWORD && sleep 3600"]

    env:
    - name: DB_USER
      valueFrom:
        secretKeyRef:
          name: db-credentials
          key: DB_USER

    volumeMounts:
    - name: secret-vol
      mountPath: /etc/db-credentials
      readOnly: true

  volumes:
  - name: secret-vol
    secret:
      secretName: db-credentials
    
#  Verify
kubectl logs secret-demo

# Expected Output
DB_USER=admin
s3cureP@ssw0rd

# Key Learning
Secret values are base64 in YAML
Inside Pod → automatically decoded

#  Task 6: Update ConfigMap (Live Update)
Observe automatic updates without restarting the Pod

#  Create ConfigMap
kubectl create configmap live-config --from-literal=message=hello
#  Pod YAML
apiVersion: v1
kind: Pod
metadata:
  name: live-demo
spec:
  containers:
  - name: test-container
    image: busybox
    command: ["sh", "-c", "while true; do cat /config/message; sleep 5; done"]

    volumeMounts:
    - name: config-vol
      mountPath: /config

  volumes:
  - name: config-vol
    configMap:
      name: live-config
    
#  Run
kubectl apply -f live-pod.yaml
kubectl logs -f live-demo

#  Update ConfigMap
kubectl patch configmap live-config \
  -p '{"data":{"message":"world"}}'
  
# Expected Behavior
Output changes from hello → world
No pod restart required

# Important Concept
Volume-mounted ConfigMaps auto-update
Environment variables do NOT update

#  Task 7: Cleanup
# Remove all created resources

#  Commands
kubectl delete pod env-demo nginx-demo secret-demo live-demo
kubectl delete configmap app-config nginx-config live-config
kubectl delete secret db-credentials

#  Verify
kubectl get all
#  Expected Output
No resources found

#  Final Summary

# I  successfully learned:
Creating ConfigMaps (literal & file)
Using ConfigMaps (env & volume)
Creating and decoding Secrets
Using Secrets securely
Live updates in Kubernetes
Cleanup best practices

#  Pro Tip
Use ENV → for simple values
Use Volumes → for config files
Use Secrets → for sensitive data
