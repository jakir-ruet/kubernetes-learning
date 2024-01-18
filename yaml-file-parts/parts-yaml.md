1. Metadata
2. Specification


# Declaration -------------------------
apiVersion: apps/v1
kind: Deployment
# -------------------------------------
# Metadata ----------------------------
metadata:
  name: nginx-deployment
  labels:
    app: nginx
# ------------------------------------
# Specification ----------------------
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.25.3
        ports:
        - containerPort: 8080
# ------------------------------------