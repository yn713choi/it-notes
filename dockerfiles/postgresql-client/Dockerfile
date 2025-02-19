FROM litmuschaos/postgresql-client

# 타임존 설정
ENV TZ=Asia/Seoul

# 타임존 데이터 설치 및 설정
RUN apk add --no-cache tzdata && \
    cp /usr/share/zoneinfo/$TZ /etc/localtime && \
    echo $TZ > /etc/timezone && \
    apk del tzdata

# 환경변수 설정으로 로케일 지정
ENV LANG=ko_KR.UTF-8 