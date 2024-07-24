# 需要梯子
FROM gradle:7.2.0-jdk11 as builder
ENV LOGSTASH_BRANCH_NAME=7.17
WORKDIR /build
RUN git clone --branch $LOGSTASH_BRANCH_NAME --single-branch https://github.com/elastic/logstash.git logstash && cd logstash && ./gradlew assemble
ENV ROCKETMQ_EXTERNALS_BRANCH_NAME=logstash-integration-custom
RUN git clone --branch $ROCKETMQ_EXTERNALS_BRANCH_NAME --single-branch https://github.com/imfms/rocketmq-externals.git rocketmq-externals && cd rocketmq-externals/rocketmq-logstash-integration/rocketmq-logstash-output && echo LOGSTASH_CORE_PATH=/build/logstash/logstash-core > gradle.properties && ./gradlew gem
RUN cp /build/rocketmq-externals/rocketmq-logstash-integration/rocketmq-logstash-output/logstash-output-rocketmq-1.0.0.gem /

FROM logstash:7.17.21
COPY --from=builder /logstash-output-rocketmq-1.0.0.gem /
RUN bin/logstash-plugin install --no-verify --local /logstash-output-rocketmq-1.0.0.gem
