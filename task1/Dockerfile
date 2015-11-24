FROM ubuntu
MAINTAINER aakher@ncsu.edu

RUN echo "Hello DevOps!" > my_created_file.txt

RUN apt-get update && \
    apt-get install -y socat && \
    rm -r /var/cache

EXPOSE 9001

CMD socat TCP4-LISTEN:9001,fork,reuseaddr SYSTEM:"cat my_created_file.txt" | sh
