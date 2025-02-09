# Eclipse Temurin JDK 17 (Alpine 기반) 이미지 사용
FROM eclipse-temurin:17-jdk-alpine

# Gradle 환경 변수 설정
ENV GRADLE_HOME=/opt/gradle

# Gradle 전용 사용자 및 그룹 생성
RUN set -o errexit -o nounset \
    && echo "Adding gradle user and group" \
    && addgroup --system --gid 1000 gradle \
    && adduser --system --ingroup gradle --uid 1000 --shell /bin/ash gradle \
    && mkdir /home/gradle/.gradle \
    && chown -R gradle:gradle /home/gradle \
    && chmod -R o+rwx /home/gradle \
    \
    && echo "Symlinking root Gradle cache to gradle Gradle cache" \
    && ln -s /home/gradle/.gradle /root/.gradle

# Gradle 캐시 유지 (Docker 볼륨 설정)
VOLUME /home/gradle/.gradle

# 작업 디렉토리 설정
WORKDIR /infra/1_docker_basic/back

# 버전 관리 시스템 (Git, Mercurial, SVN) 설치
RUN set -o errexit -o nounset \
    && echo "Installing VCSes" \
    && apk add --no-cache \
      git \
      git-lfs \
      mercurial \
      subversion \
    \
    && echo "Testing VCSes" \
    && which git \
    && which git-lfs \
    && which hg \
    && which svn

# Gradle 다운로드 및 설치
ENV GRADLE_VERSION=8.12.1
ARG GRADLE_DOWNLOAD_SHA256=8d97a97984f6cbd2b85fe4c60a743440a347544bf18818048e611f5288d46c94
RUN set -o errexit -o nounset \
    && echo "Downloading Gradle" \
    && wget --no-verbose --output-document=gradle.zip "https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip" \
    \
    && echo "Checking Gradle download hash" \
    && echo "${GRADLE_DOWNLOAD_SHA256} *gradle.zip" | sha256sum -c - \
    \
    && echo "Installing Gradle" \
    && unzip gradle.zip \
    && rm gradle.zip \
    && mv "gradle-${GRADLE_VERSION}" "${GRADLE_HOME}/" \
    && ln -s "${GRADLE_HOME}/bin/gradle" /usr/bin/gradle

# 일반 사용자 권한으로 변경
USER gradle

# Gradle 설치 확인
RUN set -o errexit -o nounset \
    && echo "Testing Gradle installation" \
    && gradle --version

# Root 사용자 권한으로 복귀
USER root

# 프로젝트 소스 코드 복사
COPY . .

# Gradle 빌드 수행 (테스트 생략)
RUN /usr/bin/gradle build -x test

# JAR 파일 실행
CMD ["java", "-jar", "build/libs/back-0.0.1-SNAPSHOT.jar"]

