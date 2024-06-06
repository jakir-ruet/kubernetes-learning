Init Container is a special type of container that runs before the main application containers in a Pod. They are used to perform initialization tasks that must be completed before the main application containers can start. 

Features
- It run one at a time and in sequence. Each init container must complete successfully before the next one starts. If any init container fails, Kubernetes will restart the Pod and re-run all the init containers.
- It can use different images and environments compared to the main application containers.
- It have access to the same network and storage as the main application containers.
- allow separation of initialization logic from the main application logic. 

User case
- Fetching configuration files or secrets from external sources, preparing them for the main application containers.
- Running database schema migrations or other preparatory database tasks before the application starts.
- Ensuring that certain services or dependencies are available and ready before the main application starts.
- Downloading or installing dependencies required by the main application that are not included in the main container image.