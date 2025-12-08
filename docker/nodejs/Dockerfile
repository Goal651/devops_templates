# This is the base image for creating our image
FROM node:alpine 

# This is changing the directory we are in
WORKDIR /app

# This is copying the package.json and package-lock.json files to the /app directory
COPY package.json package-lock.json ./

# Installing needed packages for our application
# RUN npm install 

# Installing needed packages for our application faster and more consistent
RUN npm ci

# Copying the rest of the application files to the /app directory
COPY . . 

# Exposing the port our application runs on(labelling)
EXPOSE 3000 

#For many ports
EXPOSE 3000 4000 5000 

#Running the application
CMD [ "npm","start" ]