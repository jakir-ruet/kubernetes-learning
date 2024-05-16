FROM node:20
WORKDIR /app
COPY package.json .
RUN npm install
COPY . /app
EXPOSE 3000
CMD ["node", "app.mjs"]


# run the command
# docker build .
# docker run ImgID (Above Not Working due to not publish & port expose)
# IP:3000
# docker stop  ImgID Not Working
# docker run -p 3000:3000 ImgId (Working)
# IP:3000
# docker run -p 3000:3000 -d ImgId (Working)
# IP:3000
# docker exec -it hello-node-app /bin/bash
# docker -p run exec -it hello-node /bin/bash