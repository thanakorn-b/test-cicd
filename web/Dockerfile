# pull official base image
FROM node:15.14.0 as build-stage

# set working directory
WORKDIR /app

# Add `/app/node_modules/.bin` to $PATH
ENV PATH /app/node_modules/.bin:$PATH

# Install app dependencies
COPY package.json /app/
COPY yarn.lock /app/
RUN yarn install

# Build production mode
COPY . .
RUN yarn build
RUN  ls -l
# Production stage
FROM nginx:alpine-perl as production-stage

RUN rm -rf /etc/nginx/conf.d/default.conf
COPY --from=build-stage  /app/nginx/default.conf /etc/nginx/conf.d/default.conf
COPY --from=build-stage /app/build /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]