FROM debian

#Tools
RUN apt-get -y update && apt-get install -y python3 python3-pip python3-bsddb3 intltool git python3-gi python3-pil python3-icu

#Gramps
RUN git clone --depth 1 https://github.com/gramps-project/gramps.git
RUN cd gramps/ && python3 setup.py build
RUN cd gramps/ && python3 setup.py install

#Gramps Database Backends
RUN mkdir .gramps
RUN mkdir .gramps/gramps50
RUN cd .gramps/gramps50 && git clone --depth 1 https://github.com/gramps-project/addons-source.git
RUN cd .gramps/gramps50 && mv addons-source plugins
RUN cd .gramps/gramps50 && rm -rf ~/.gramps/gramps50/plugins/*Pack
RUN cd .gramps/gramps50 && rm -rf ~/.gramps/gramps50/plugins/libwebconnect

#Sample Database
RUN cd ~ && ln -s /.gramps
RUN gramps --config behavior.database-backend:dbapi -C "GrampsConnect" -i /gramps/example/gramps/example.gramps 

#Gramps Connect
# Requires editing of the security group to add InBound HTTP
RUN git clone --depth 1 https://github.com/gramps-project/gramps_connect.git
RUN cd gramps_connect/ && pip3 install tornado
EXPOSE 8085
ENTRYPOINT cd gramps_connect/ && PYTHONPATH=. python3 gramps_connect/app.py --database='GrampsConnect' --port=8085

#Slimming down container
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
