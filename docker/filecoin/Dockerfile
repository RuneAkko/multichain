FROM ubuntu:xenial

RUN apt update -y
RUN apt install -y mesa-opencl-icd ocl-icd-opencl-dev gcc git bzr jq pkg-config curl wget nano
RUN apt upgrade -y

RUN wget -c https://golang.org/dl/go1.14.6.linux-amd64.tar.gz
RUN tar -C /usr/local -xzf go1.14.6.linux-amd64.tar.gz
ENV PATH=$PATH:/usr/local/go/bin

COPY run.sh /root/run.sh
COPY miner.key /root/miner.key
RUN chmod +x /root/run.sh
RUN chmod +x /root/miner.key

WORKDIR /app

RUN git clone https://github.com/filecoin-project/lotus .
RUN make 2k
RUN ./lotus fetch-params 2048
RUN ./lotus-seed pre-seal --sector-size 2KiB --num-sectors 2
RUN ./lotus-seed genesis new localnet.json
RUN ./lotus-seed genesis add-miner localnet.json ~/.genesis-sectors/pre-seal-t01000.json

EXPOSE 1234

CMD /root/run.sh
