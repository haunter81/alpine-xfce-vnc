FROM alpine:3.12

ENV DISPLAY :1
# alternative 1024x768x16
ENV RESOLUTION 1920x1080x24
ENV PASS="pass123"

# setup desktop environment (xfce4), display server (xvfb), vnc server (x11vnc)
RUN apk add --no-cache \
  xfce4 \
  faenza-icon-theme \
  xvfb \
  x11vnc \
  xfce4-terminal

# setup novnc (requires bash)
RUN apk add --no-cache --repository http://dl-cdn.alpinelinux.org/alpine/edge/testing \
  bash \
  novnc && \
  ln -s /usr/share/novnc/vnc.html /usr/share/novnc/index.html

RUN echo "http://nl.alpinelinux.org/alpine/edge/main" >> /etc/apk/repositories
RUN echo "http://nl.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories
RUN echo "http://nl.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories

# setup supervisor
COPY supervisor /tmp
SHELL ["/bin/bash", "-c"]
RUN apk add --no-cache supervisor && \
  echo_supervisord_conf > /etc/supervisord.conf && \
  sed -i -r -f /tmp/supervisor.sed /etc/supervisord.conf && \
  mkdir -pv /etc/supervisor/conf.d /var/log/{novnc,x11vnc,xfce4,xvfb} && \
  mv /tmp/supervisor-*.ini /etc/supervisor/conf.d/ && \
  rm -fr /tmp/supervisor*


#import start
RUN \
	apk --no-cache add \
		desktop-file-utils \
		exo \
		garcon \
		gtk+2.0 \
		libxfce4ui \
		libxfce4util \
		thunar \
		thunar-archive-plugin \
		ttf-dejavu \
		ttf-freefont \
		xarchiver \
		xdotool \
		xfce4-appfinder \
		xfce4-panel \
		xfce4-settings \
		xfce4-terminal \
		xfconf \
		xfdesktop \
		xterm \
		yad

RUN \
	apk --no-cache add \
		bash \
		bash-completion \
		bash-doc \
		ca-certificates \
		curl \
		dbus \
		dbus-x11 \
		eudev \
		ffmpeg \
		ffmpeg-libs \
		fuse \
		git \
		htop \
		java-jna \
		libgcc \
		libmediainfo \
		libstdc++ \
		mandoc \
		mandoc-apropos \
		mandoc-soelim \
		man-pages \
		mc \
		mesa-dri-swrast \
		mediainfo \
		nano \
		nss \
		openjdk8-jre \
		p7zip \
		p7zip-doc \
		python3 \
		py3-pip \
		py3-qt5 \
		qt5-qtbase \
		qt5-qtsvg \
		rsync \
		rtmpdump \
		screen \
		sudo \
		tar \
		tar-doc \
		tmux \
		unrar \
		unzip \
		util-linux \
		vim \
		wget \
		wxgtk \
		xz \
		xz-doc \
		zip \
	&& update-ca-certificates

EXPOSE 5900

CMD ["supervisord", "-c", "/etc/supervisord.conf", "-n"]
