FROM thespaghettidetective/web:base-1.1

ARG uid=0
ARG gid=0
ARG user=root
ARG group=root
ARG with_node=0

RUN if [[ "$with_node" == 1 ]]; then \
    apk add --no-cache --update inotify-tools nodejs npm && npm install -g yarn \
    ; fi

RUN if [[ "$uid" != 0 ]]; then \
    addgroup --gid $gid $group && \
    adduser --disabled-password --uid $uid --ingroup $group $user && \
    mkdir -p /home/$user /home/$user/.cache /home/$user/.ipython /home/$user/.local && \
    mkdir -p /app/ /app/frontend /app/static_build && \
    chown -R $user:$group /app /home/$user \
    ; fi

USER $user

EXPOSE 3334
ADD . /app
RUN if [[ "$uid" != 0 ]]; then \
    pip install -U pip && pip install -r requirements.txt --src /home/$user/.local/src \
    ; else \
    pip install -U pip && pip install -r requirements.txt \
    ; fi


VOLUME /app
VOLUME /home/$user/.cache
VOLUME /home/$user/.ipython
VOLUME /home/$user/.local

WORKDIR /app

ENV PYTHONPATH "/home/$user/.local:${PYTHONPATH}"
ENV PATH "/home/$user/.local/bin:${PATH}"
