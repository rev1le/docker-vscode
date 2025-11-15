FROM ubuntu:noble

RUN apt update

# https://code.visualstudio.com/docs/setup/linux
RUN apt install -y wget gpg
RUN wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
RUN install -D -o root -g root -m 644 microsoft.gpg /usr/share/keyrings/microsoft.gpg
RUN rm -f microsoft.gpg

COPY vscode.sources /etc/apt/sources.list.d/vscode.sources

RUN apt install -y apt-transport-https
RUN apt update
RUN apt install -y code

RUN apt clean

RUN useradd -m -u 1001 vscode

USER vscode

WORKDIR /home/vscode

EXPOSE 80

CMD ["code", "serve-web", "--without-connection-token", "--accept-server-license-terms", "--host", "0.0.0.0", "--port", "80", "--server-data-dir", "/home/vscode/data/.vscode-server-web", "--server-base-path", "/home/vscode/projects"]
