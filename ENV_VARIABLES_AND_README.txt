#------------------------
# ARG's should be added during build command with "--build-arg" command.
# e.g. "docker build -t image/name -f /docker/file/location/dockerfilename --build-arg ARG_TO_PASS=target/iksde.jar ."
# ENV variables should be added when image is about to run.
# e.g. "docker run -d -p 2000:2000 -p 2001:2001 -e PROXY_PORT=2000 -e SERVER_PORT=2001 mock/image/name"

# Important note: Paths passed through docker's e.g. COPY command are relative to place, where dockerfile resides. Thus "C:/Users/youruser/Desktop/awesomeapp/target/app.jar" wont work.
# Important note#2: password to connect to firefoxes via rdp are "secret".
# ------------------------

# ===========================================Dockerfile_create_mock_image===========================================
# Put this dockerfile to your project dir and use relative path to war file
# Sample build command: "docker build -t image/name -f Dockerfile_create_mock_image ."
# Sample run command (lazy): "docker run -p 1080:1080 -d tag_used_in_build_command" # <--- this will work as long as your jar file name is the same as stated in dockerfile (!DURING BUILD!) if it does not, just add "-e APPLICATION_JAR" switch pointing to your jar
# Sample run command: "docker run -d -p 1234:1234 -p 2345:2345 -e PROXY_PORT=1234 -e SERVER_PORT=2345 -e APPLICATION_JAR=relative/path/from/place/you/build/dockerfile/file.jar tag_used_in_build_command"
ENV PROXY_PORT=1080
ENV SERVER_PORT=1090
ENV APPLICATION_JAR=target/testingcup.jar  # <-- this is an actual place in Docker container where jar exists. It is copied with command "COPY . ."



===========================================dockerfile_create_wildfly_server_and_deploy_app===========================================
# Put this dockerfile to your project dir and use relative path to war file
# Sample build command "docker build -t tag_for_image -f dockerfile_create_wildfly_server_and_deploy_app --build-arg APPLICATION_WAR=relative/path/from/place/you/build/dockerfile/file.war ."
# Sample run command "docker run -d -p 8080:8080 tag_used_in_build_command"

ARG APPLICATION_WAR=target/test.war

# DEPLOYMENT_DIR defined in parent image - do not touch it if you want your application to be automatically deployed when container starts


#===========================================dockercompose.yml===========================================
# All ENV variables can be modified in the file
# ARG variables i.e APPLICATION_WAR must be equal to file that is going to be deployed to server. Else, server will start, but application will not be deployed.
# Sample up command: docker-compose -f dockercompose.yml up -d --build
# Sample dowon command: docker-compose -f dockercompose.yml down