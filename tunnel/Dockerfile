FROM alpine:3.2
LABEL Maintainer="Nikolay Volkov <gitkv@ya.ru>" \
      Description="SSH tunnel based on Alpine Linux."

RUN apk add --update openssh-client && rm -rf /var/cache/apk/*

CMD rm -rf /root/.ssh \
    && mkdir /root/.ssh \
    && cp -R /root/ssh/* /root/.ssh/ \
    && chmod -R 600 /root/.ssh/* \
    && ssh -vv -o StrictHostKeyChecking=no -N -L \
        *:$LOCAL_PORT:$REMOTE_HOST:$REMOTE_PORT $SSH_USER@$SSH_HOST -p $SSH_PORT \
    && while true; do sleep 30; done;

EXPOSE 1-65535
