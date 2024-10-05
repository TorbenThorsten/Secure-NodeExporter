# Secure-NodeExporter
This is a Dockerized Solution to expose a Node Exporter securely over the Internet.

It's pretty simple and I just threw in a few configs to get an NGINX reverse Proxy handling the requests of your Prometheus Server. It is also configured to use a Basic Authentication and HTTPS.

You can use Self signed Certificates e.g. using the command:

    openssl req -new -x509 -key ssl_cert/nginx-selfsigned.key -out ssl_cert/nginx-selfsigned.crt -days 365 -subj "/CN=YOURHOST"

Just put these, or any other, in the ssl_cert folder and your HTTPS should work.

To add the Password to the .htpasswd Just use the following command:

    htpasswd -c .htpasswd <username>

__Change the Password before using the containers in productive enviroment!!!__

Install apache2-utils if needed

    sudo apt-get install apache2-utils


### Troubleshooting:

Maybe you need to change your Prometheus Config to match the following example Pattern:

    # Example job for node_exporter
    - job_name: 'node_exporter'
        scheme: https
        static_configs:
            - targets: ['Your.Hostname:8443']
        basic_auth:
            username: mon
            password: Password
        tls_config:
            insecure_skip_verify: true