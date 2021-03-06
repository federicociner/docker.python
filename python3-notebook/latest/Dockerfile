FROM federicociner/python3-base:latest
MAINTAINER Federico Ciner <federico.ciner@gmail.com>

ARG DEBIAN_FRONTEND=noninteractive

ENV SHELL /bin/bash
ENV NB_USER notebook
ENV NB_UID 1000
ENV HOME /home/$NB_USER
ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8

# Create notebook user with UID=1000 and in the 'users' group
RUN useradd -m -s /bin/bash -N -u $NB_UID $NB_USER

USER $NB_USER

# Setup work directory for backward-compatibility
RUN mkdir /home/$NB_USER/work

USER root

RUN pip3 install jupyter && pip3 install jupyterlab
RUN jupyter serverextension enable --py jupyterlab --sys-prefix

RUN echo "root:password" | chpasswd
RUN echo "notebook:password" | chpasswd

RUN adduser notebook sudo

EXPOSE 8888
WORKDIR $HOME

# Configure container startup
ENTRYPOINT ["tini", "--"]
CMD ["start.sh"]

COPY jupyter_notebook_config.py /etc/jupyter/
RUN chown -R $NB_USER:users /etc/jupyter/

# Switch back to jovyan to avoid accidental container runs as root
USER $NB_USER

# Add local files as late as possible to avoid cache busting
COPY start.sh /usr/local/bin/
