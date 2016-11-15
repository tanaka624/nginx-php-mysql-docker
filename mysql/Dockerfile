FROM mysql:5.7

RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y tzdata
RUN cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
RUN apt-get clean && rm -rf /var/cache/apt/archives/* /var/lib/apt/lists/*
