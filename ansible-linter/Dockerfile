FROM python:3

RUN pip3 install "ansible-lint"

ARG DEFAULT_COMMAND='--help'

ENV COMMAND $DEFAULT_COMMAND

#docker run -v "/home/vasiliy/development/ansible-deploy/:/app" -e COMMAND="-p /app/roles/docker-image-sites/tasks/sites.yaml" linter
CMD ["sh", "-c", "ansible-lint $COMMAND"]
