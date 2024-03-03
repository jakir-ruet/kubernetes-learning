FROM node:20
WORKDIR /app
COPY package.json .
RUN npm install
COPY . /app
EXPOSE 3000
CMD ["node", "app.mjs"]


# run the command
# docker build .
# docker run  c7bbfc10fb91d5c24eee150639c6077bd15abab67116bd6403b939f3bdc1678e
# localhost:3000
# docker exec -it hello-node-app /bin/bash
# docker -p run exec -it hello-node /bin/bash