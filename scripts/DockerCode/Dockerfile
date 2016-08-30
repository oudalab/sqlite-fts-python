FROM python
FROM ubuntu
COPY . /src
RUN apt-get update &&\
    apt-get -y upgrade
RUN apt-get install -y git &&\
    apt-get install -y curl &&\
    apt-get install -y tcl &&\
    apt-get install -y python3-pip &&\
    apt-get install -y vim htop strace tmux
RUN pip3 install --upgrade pip &&\
    pip3 install sqlitefts &&\
    pip3 install apsw &&\
    pip3 install nltk &&\
    pip3 install cltk
RUN cd /src &&\
    ./buildcode
    



