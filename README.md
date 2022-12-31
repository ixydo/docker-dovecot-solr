# Requirments

1. docker-compose-plugin
2. python3 (optional, for generating password hashes)

# Quickstart

1. Start the solr service

       docker compose up -d

2. Configure dovecot to use solr for full text search (fts)

# Authentication

By default Apache Solr is set up with open access to all.

There's an sample basic auth configuration you can use to protect this instance.

1. Using `var_solr/data/security-sample.json` as a basis activate
   authentication in Solr by creating the `security.json` configuration file.

       cp var_solr/data/security-sample.json var_solr/data/security.json

2. Generate auth credentials using the `solr-password-hasher` script in this
   repo.

       ./solr-passwd-hasher

3. Take the last line of output from `solr-passwd-hasher` and update the
   `.authentication.credentials.dovecot` value in `var_solr/data/security.json`.

   You can create additional user/pass pairs by extending the `credentials`
   JSON object in `var_solr/data/security.json`.

4. Configure Dovecot to use basic auth by updating the `fts_solr` URL.

   For example, where the connection without authentication is:

       fts_solr = url=http://localhost:8983/solr/dovecot/

   with authentication this would be:

       fts_solr = url=http://dovecot:password@localhost:8983/solr/dovecot/

# Docker Compose Configuration

You can manage a couple of parameters in a local `.env` file that will be used
by docker compose when starting up the container:

| Variable | Description |
|---|---|
| PORTS | IP & port mapping for docker (defaults to `8983:8983`) |
| JAVA_MIN_MEMORY | Minimum heap size for the JVM (defaults to 512m) |
| JAVA_MAX_MEMORY | Maxiumum heap size for the JVM (defaults to 512m) |
