FROM mcr.microsoft.com/devcontainers/php:1-8.3

# Install MariaDB client
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get install -y mariadb-client \
    && apt-get clean -y && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install mysqli pdo pdo_mysql

# Install Tideways PHP extension
COPY --from=ghcr.io/tideways/php:latest /tideways/ /tideways/
RUN docker-php-ext-enable --ini-name tideways.ini "$(php /tideways/get-ext-path.php)"

ENV TIDEWAYS_SERVICE=app
ENV TIDEWAYS_TRACE_SAMPLE_RATE=25
ENV TIDEWAYS_CONNECTION=tcp://tideways-daemon:9135

RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar && chmod +x wp-cli.phar && mv wp-cli.phar /usr/local/bin/wp

# [Choice] Node.js version: none, lts/*, 22, 20, 18
ARG NODE_VERSION="22"
RUN if [ "${NODE_VERSION}" != "none" ]; then su vscode -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"; fi

# [Optional] Uncomment this section to install additional OS packages.
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>

# [Optional] Uncomment this line to install global node packages.
# RUN su vscode -c "source /usr/local/share/nvm/nvm.sh && npm install -g <your-package-here>" 2>&1

RUN yes | npx playwright install-deps  
RUN npx playwright install 
