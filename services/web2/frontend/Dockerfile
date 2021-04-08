# pull official base image
FROM node:10.13.0-slim

# set work directory
WORKDIR /usr/src/app/frontend


# copy project
COPY . /usr/src/app/frontend

RUN npm install 
RUN npm build 

CMD ["npm", "run", "dev"]
