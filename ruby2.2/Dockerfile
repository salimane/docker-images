FROM salimane/centos7:latest
MAINTAINER Salimane Adjao Moustapha <me@salimane.com>

ENV RUBY_MAJOR 2.2
ENV RUBY_VERSION 2.2.2
ENV RUBY_DOWNLOAD_SHA256 5ffc0f317e429e6b29d4a98ac521c3ce65481bfd22a8cf845fa02a7b113d9b44
ENV RUBYGEMS_MAJOR 2.4
ENV RUBYGEMS_VERSION 2.4.8
ENV BUNDLER_VERSION 1.10.5
ENV GEM_HOME /usr/local/bundle
ENV PATH $GEM_HOME/bin:$PATH
ENV BUNDLE_APP_CONFIG $GEM_HOME
ENV LANG en_US.utf8

ENV RUNTIME_DEPS "readline zlib libyaml openssl bzip2 make"
ENV BUILD_DEPS "gcc-c++ readline-devel zlib-devel libyaml-devel libffi-devel openssl-devel patch autoconf automake libtool bison iconv-devel"

# Store list of installed RPM's
# Install dependencies
# Install Ruby
# Uninstall build dependencies to avoid image bloat
# Make sure runtime dependencies are still present
# Clean up
RUN yum -y update \
		&& rpm -qa --queryformat "%{NAME}\n" > /tmp/rpm.before \
		&& yum -y install $RUNTIME_DEPS $BUILD_DEPS \
		&& mkdir -p /usr/src/ruby \
		&& curl -fSL -o ruby.tar.gz "http://cache.ruby-lang.org/pub/ruby/$RUBY_MAJOR/ruby-$RUBY_VERSION.tar.gz" \
		&& echo "$RUBY_DOWNLOAD_SHA256 *ruby.tar.gz" | sha256sum -c - \
		&& tar -xzf ruby.tar.gz -C /usr/src/ruby --strip-components=1 \
		&& rm ruby.tar.gz \
		&& cd /usr/src/ruby \
		&& autoconf \
		&& ./configure --disable-install-doc \
		&& make -j"$(nproc)" \
		&& make install \
		&& echo 'gem: --no-document' > /usr/local/etc/gemrc \
		&& rpm -qa --queryformat "%{NAME}\n" > /tmp/rpm.after \
		&& diff /tmp/rpm.before /tmp/rpm.after | grep '^[<>] ' | cut -d ' ' -f 2 | sort | uniq -c | grep '^\s*1 ' | tr -s ' ' ' ' | cut -d ' ' -f 3 | xargs yum -y erase || true \
		&& yum -y install $RUNTIME_DEPS \
		&& yum clean all \
		&& rm -r /usr/src/ruby

# Install rubygems and bundler
RUN mkdir -p /usr/src/rubygems \
		&& cd /tmp \
		&& curl -fSL -o rubygems.tgz "http://production.cf.rubygems.org/rubygems/rubygems-$RUBYGEMS_VERSION.tgz" \
		&& tar -zxf rubygems.tgz -C /usr/src/rubygems --strip-components=1 \
		&& rm /tmp/rubygems.tgz \
		&& cd /usr/src/rubygems \
		&& ruby setup.rb \
		&& /bin/bash -l -c "gem install bundler --version '$BUNDLER_VERSION' --no-rdoc --no-ri" \
		&& /bin/bash -l -c "bundle config --global path '$GEM_HOME'" \
		&& /bin/bash -l -c "bundle config --global bin '$GEM_HOME/bin'" \
		&& echo "gem: --no-ri --no-rdoc" > ~/.gemrc \
		&& rm -r /usr/src/rubygems

CMD [ "bash" ]
