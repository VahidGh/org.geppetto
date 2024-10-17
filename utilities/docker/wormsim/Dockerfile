FROM metacell/java-virgo-maven:development

LABEL maintainer="Facundo Rodriguez <facundo@metacell.us>"

ARG targetBranch=v0.4.2-alpha
ARG originBranch=v0.4.2-alpha
ARG defaultBranch=v0.4.2-alpha

# ARG targetBranch=v0.2.6-alpha
# ARG originBranch=v0.2.6-alpha
# ARG defaultBranch=v0.2.6-alpha

RUN /bin/echo -e "\e[1;35mORIGIN BRANCH ------------ $originBranch\e[0m" &&\
  /bin/echo -e "\e[1;35mTARGET BRANCH ------------ $targetBranch\e[0m" &&\
  /bin/echo -e "\e[1;35mDEFAULT BRANCH ------------ $defaultBranch\e[0m" 


# # Test: Adding wormsim v0.2.6 to check for possible incompatibilities
# USER root

# RUN rm -rf /var/lib/apt/lists/*
# RUN rm -rf /etc/apt/sources.list/*
# RUN rm -rf /etc/apt/sources.list.d/*

# COPY sources.list /etc/apt/sources.list

# RUN apt-get -o Acquire::Check-Valid-Until=false -y --force-yes update &&\
#   apt-get -o Acquire::Check-Valid-Until=false -y --force-yes upgrade &&\
#   apt-get install --no-install-recommends npm -o Acquire::Check-Valid-Until=false -y --force-yes

# RUN npm install -g bower
# RUN ln -s /usr/bin/nodejs /usr/bin/node
# RUN npm cache clean --force
  
# USER developer

  
# get geppetto
RUN mkdir -p workspace &&\
  cd workspace &&\
  ../copy.sh http://github.com/openworm/org.geppetto.git "${targetBranch}" "${originBranch}" "${defaultBranch}"

WORKDIR $HOME/workspace
RUN ../copy.sh https://github.com/openworm/org.geppetto.model.git "${targetBranch}" "${originBranch}" "${defaultBranch}" &&\
  cd org.geppetto.model &&\
  /bin/echo -e "\e[96mMaven install org.geppetto.model\e[0m" &&\
  mvn -DskipTests --quiet install &&\
  rm -rf src

RUN ../copy.sh https://github.com/openworm/org.geppetto.core.git "${targetBranch}" "${originBranch}" "${defaultBranch}" &&\
  cd org.geppetto.core &&\
  /bin/echo -e "\e[96mMaven install org.geppetto.core\e[0m" &&\
  mvn -DskipTests --quiet install &&\
  rm -rf src

RUN ../copy.sh https://github.com/openworm/org.geppetto.model.neuroml.git "${targetBranch}" "${originBranch}" "${defaultBranch}" &&\
  cd org.geppetto.model.neuroml &&\
  /bin/echo -e "\e[96mMaven install org.geppetto.model.neuroml\e[0m" &&\
  mvn -DskipTests --quiet install &&\
  rm -rf src

# # swc v0.2.6 mistakenly released as 0.2.6 in the repo
# RUN ../copy.sh https://github.com/openworm/org.geppetto.model.swc.git "0.2.6-alpha" "0.2.6-alpha" "0.2.6-alpha" &&\
#   cd org.geppetto.model.swc &&\
#   /bin/echo -e "\e[96mMaven install org.geppetto.model.swc\e[0m" &&\
#   mvn -DskipTests --quiet install &&\
#   rm -rf src

# RUN ../copy.sh https://github.com/openworm/org.geppetto.model.nwb.git "${targetBranch}" "${originBranch}" "${defaultBranch}" &&\
  # cd org.geppetto.model.nwb &&\
  # /bin/echo -e "\e[96mMaven install org.geppetto.model.nwb\e[0m" &&\
  # mvn -DskipTests --quiet install &&\
  # rm -rf src

RUN ../copy.sh https://github.com/openworm/org.geppetto.simulation.git "${targetBranch}" "${originBranch}" "${defaultBranch}" &&\
  cd org.geppetto.simulation &&\
  /bin/echo -e "\e[96mMaven install org.geppetto.simulation\e[0m" &&\
  mvn -DskipTests --quiet install &&\
  rm -rf src

RUN ../copy.sh https://github.com/openworm/org.geppetto.frontend.git "${targetBranch}" "${originBranch}" "${defaultBranch}"

# RUN cd $HOME/workspace/org.geppetto.frontend/src/main &&\
  # $HOME/copy.sh https://github.com/openworm/geppetto-application.git "${targetBranch}" "${originBranch}" "${defaultBranch}" &&\
  # mv geppetto-application webapp

# RUN cd $HOME/workspace/org.geppetto.frontend/src/main/webapp &&\
  # $HOME/copy.sh https://github.com/openworm/geppetto-client.git "${targetBranch}" "${originBranch}" "${defaultBranch}"



# # Test: Adding wormsim v0.2.6 to check for possible incompatibilities
# # RUN ../copy.sh https://github.com/openworm/org.wormsim.frontend.git "${targetBranch}" "${originBranch}" "${defaultBranch}"
# RUN ../copy.sh https://github.com/openworm/org.wormsim.frontend.git "master" "master" "master"

# WORKDIR $HOME
# COPY bower.json workspace/org.geppetto.frontend/src/main/webapp/js/
# RUN cd workspace/org.geppetto.frontend/src/main/webapp/js && bower install
# COPY components.js workspace/org.geppetto.frontend/src/main/webapp/js/

# RUN cd workspace/org.wormsim.frontend/utilities/ && sudo python applyWormSimTheme.py && sudo python deployWormSimComponents.py && sudo python deployWormSimData.py

# RUN cd $HOME/workspace/org.wormsim.frontend &&\
#   /bin/echo -e "\e[96mMaven install org.wormsim.frontend\e[0m" &&\
#   mvn -DskipTests --quiet install &&\
#   rm -rf src

# USER root
# RUN npm cache clean --force
# USER developer


WORKDIR $HOME/workspace
COPY --chown=1000:1000 GeppettoConfiguration.json $HOME/workspace/org.geppetto.frontend/src/main/webapp/
RUN cd $HOME/workspace/org.geppetto.frontend &&\
  /bin/echo -e "\e[96mMaven install org.geppetto.frontend\e[0m" &&\
  mvn -Dhttps.protocols=TLSv1.2 -DcontextPath=/ -DuseSsl=true -DskipTests -Pmaster --quiet install &&\
  rm -rf src


WORKDIR $HOME
RUN mkdir rm $SERVER_HOME/./repository/usr
COPY --chown=1000:1000 geppetto.plan $HOME/workspace/org.geppetto/
COPY --chown=1000:1000 config.json $HOME/workspace/org.geppetto/utilities/source_setup/
RUN rm $SERVER_HOME/pickup/*.jar
RUN cd $HOME/workspace/org.geppetto/utilities/source_setup && python update_server.py

EXPOSE 8080
CMD [ "/bin/bash", "-c", "$SERVER_HOME/bin/startup.sh" ]
