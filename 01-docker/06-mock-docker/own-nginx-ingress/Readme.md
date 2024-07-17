```bash
docker build . -t own-nginx-ingress
docker images
docker ps
docker ps -a
docker run -p 3000:80 -d BaseImageId
```
```bash
docker tag own-nginx-ingress jakirbd/own-nginx-ingress
docker push jakirbd/own-nginx-ingress
```