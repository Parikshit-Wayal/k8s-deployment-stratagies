# Liveness Probe
## Liveness Probe Demo 🎥

You can watch the demo video here:  


https://github.com/user-attachments/assets/cefd0185-235b-4ce6-8d70-a4b5677ab130

---

## ⚙️ Purpose

The **liveness probe** helps Kubernetes automatically detect when a container has stopped responding or become unhealthy.  
If the probe fails, Kubernetes **restarts the container** to keep the application running smoothly.

---

## 🧩 YAML Explanation

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: liveness-demo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: liveness-demo-app
  template:
    metadata:
      labels:
        app: liveness-demo-app
    spec:
      containers:
      - name: liveness-container
        image: busybox:1.36.1
        args:
        - /bin/sh
        - -c
        - touch /tmp/healthy; sleep 30000; # Create the file and keep the container running

        livenessProbe:
          exec:                    # Use the 'exec' probe type
            command:
            - cat                 # Command to check the file
            - /tmp/healthy
          initialDelaySeconds: 5   # Wait 5 seconds before first check
          periodSeconds: 5         # Check every 5 seconds
          failureThreshold: 1      # Restart container after 1 failure
```

🧠 How It Works
The container starts and creates a file /tmp/healthy.

The liveness probe runs every 5 seconds and checks that file.

If the file is missing or can’t be read, the probe fails.

When the probe fails, Kubernetes automatically restarts the container.

