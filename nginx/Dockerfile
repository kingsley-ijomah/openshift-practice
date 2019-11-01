# Base image:
# https://torstenwalter.de/openshift/nginx/2017/08/04/nginx-on-openshift.html#run_nginx_in_openshift

FROM nginx:stable

# Install dependencies
RUN apt-get update -qq && apt-get -y install apache2-utils

# for vim
RUN apt-get install -y vim
 
# establish where Nginx should look for files
ENV RAILS_ROOT /myapp
ENV APP_NAME docker_app
 
# Set our working directory inside the image
WORKDIR $RAILS_ROOT
 
# create log directory
RUN mkdir log

# support running as arbitrary user which belogs to the root group
RUN chmod g+rwx /log/myapp/log/nginx.access.log
 
# copy over static assets
COPY public public/
 
# Copy Nginx config template
COPY config/nginx.conf /tmp/docker_example.nginx
 
# substitute variable references in the Nginx config template for real values from the environment
# put the final config in its place
RUN envsubst '$RAILS_ROOT, $APP_NAME' < /tmp/docker_example.nginx > /etc/nginx/conf.d/default.conf

# support running as arbitrary user which belogs to the root group
RUN chmod g+rwx /var/cache/nginx /var/run /var/log/nginx

EXPOSE 8081

# comment user directive as master process is run as user in OpenShift anyhow
RUN sed -i.bak 's/^user/#user/' /etc/nginx/nginx.conf
 

# Use the "exec" form of CMD so Nginx shuts down gracefully on SIGTERM (i.e. `docker stop`)
CMD [ "nginx", "-g", "daemon off;" ]
