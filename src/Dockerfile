FROM alpine:3.19
MAINTAINER tess@ten7.com

# Update the package list and install Ansible.
RUN apk -U upgrade &&\
    apk add --update --no-cache ansible-core git && \
    rm -rf /tmp/* \
           /var/cache/apk/*

# Copy the Ansible configuration files
COPY ansible-hosts /etc/ansible/hosts
COPY ansible.cfg /etc/ansible/ansible.cfg
COPY ansible /ansible

# Run the build.
RUN ansible-galaxy install -fr /ansible/requirements.yml && \
    ansible-playbook -i /ansible/inventories/app.yml /ansible/build.yml

# Switch to the service account.
USER app

# Set the entrypoint and default command.
ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["kubectl"]

