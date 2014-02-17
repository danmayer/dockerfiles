# docker image base for ruby apps

FROM ubuntu

RUN export DEBIAN_FRONTEND=noninteractive && apt-get -y update

#make the base sshable
RUN apt-get install -y openssh-server
RUN mkdir /var/run/sshd
RUN echo "root:password" | chpasswd
EXPOSE 22

RUN apt-get -y install wget git emacs curl

# install Ruby 1.9.3-p484
RUN apt-get -y install build-essential zlib1g-dev libreadline-dev libssl-dev libcurl4-openssl-dev -f
RUN git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
RUN git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
RUN echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
RUN echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
ENV PATH /.rbenv/bin:/.rbenv/shims:$PATH
RUN echo PATH=$PATH
RUN rbenv init -
RUN rbenv install 1.9.3-p484 && rbenv global 1.9.3-p484

# Install common gem dependancies
RUN apt-get -y install libxslt1-dev libxml2-dev libsasl2-2 libsasl2-dev redis-server memcached libmysqlclient-dev mysql-client

# never install a ruby gem docs
RUN echo "gem: --no-rdoc --no-ri" >> ~/.gemrc

# Install bundler and the "bundle" shim
RUN gem install bundler && rbenv rehash && bundle config --global --jobs 8 && rbenv rehash

# Command to run at "docker run ..."
CMD ["/bin/bash", "--login"]
