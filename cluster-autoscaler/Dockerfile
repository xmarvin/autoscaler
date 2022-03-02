FROM golang:1.17.5 as builder

ENV GOPATH /gopath/
ENV PATH $GOPATH/bin:$PATH
ENV GO111MODULE auto

RUN apt-get update && apt-get --yes install libseccomp-dev
RUN go version
RUN go get github.com/tools/godep
RUN godep version
RUN mkdir /src
ADD . /src
WORKDIR /src
RUN CGO_ENABLED=0 GOOS=linux go build -o cluster-autoscaler

FROM scratch

COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
COPY --from=builder /usr/share/zoneinfo /usr/share/zoneinfo/
COPY --from=builder /src/cluster-autoscaler /cluster-autoscaler

CMD ["/cluster-autoscaler"]
