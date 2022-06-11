# Use a 'node:alpine' image to run the npm build.
FROM node:18.3.0-alpine as builder
WORKDIR '/app'
COPY package.json .
RUN npm install
COPY . .
RUN npm run build

# Use an 'nginx' image to host the application.
# Web content is copied over from the previous container's /app/build folder.
FROM nginx
# The following is used by Elastic Beanstalk to expose port 3000
EXPOSE 3000
COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /app/build /usr/share/nginx/html

# No need to specify CMD (default command will start nginx for us)