# Docker 이미지를 구성하기 위해 베이스로 nodejs 최신 버전 사용
FROM node:latest

# 작업 디렉토리 설정
WORKDIR ~/infra/1_docker_basic/front

# package.json, package-lock.json 관련
COPY package*.json ./
RUN npm install

# Dockerfile이 위치한 디렉토리 모든 파일과 디렉토리를 컨테이너의 작업 디렉토리로 복사
COPY . .

# npm run build 수행
RUN npm run build

# http 서버를 전역으로 설치 후 빌드된 정적 파일을 호스팅
RUN npm install -g http-server

# 8080 열어두기
EXPOSE 8080

CMD ["http-server", "dist"]
