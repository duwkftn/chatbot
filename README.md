# chatbot — Flask 앱 컨테이너화와 쿠버네티스 배포 실습

클라우드 인프라 관리 수업에서 만든 실습입니다. 간단한 Flask 웹 앱을 Docker 이미지로 만들고, 쿠버네티스 Deployment와 Service(NodePort)로 배포합니다.

**직무:** 클라우드 인프라 운영  ·  **기술:** Python Flask, Docker, Kubernetes

## 이 실습에서 한 것

- Flask 앱(`app.py`)을 `python:3.11-slim` 기반 이미지로 빌드하는 `Dockerfile`
- 이미지를 Docker Hub(`duwkftn/chatbot`)에 올리고, `deployment.yaml`로 쿠버네티스에 배포
- Service를 NodePort로 열어 클러스터 밖에서 5000번 포트로 접속
- 응답 문구의 `(auto deployed)`는 코드를 바꿔 다시 배포했을 때 새 버전이 반영됐는지 확인하려고 넣은 표시입니다

## 파일

| 파일 | 내용 |
|---|---|
| `app.py` | `/` 요청에 안내 문구를 돌려주는 Flask 앱 (0.0.0.0:5000) |
| `Dockerfile` | 이미지 빌드 정의 |
| `deployment.yaml` | Deployment(replicas 1) + Service(NodePort, 5000) |

## 실행

```bash
docker build -t duwkftn/chatbot:latest .
docker run -p 5000:5000 duwkftn/chatbot:latest      # 로컬 확인: http://localhost:5000

kubectl apply -f deployment.yaml                      # 쿠버네티스 배포
kubectl get svc chatbot                               # 할당된 NodePort 확인
```

## 더 해 볼 것

- 이미지 태그를 `latest` 대신 버전으로 고정하고, 컨테이너를 root가 아닌 사용자로 실행
- Deployment에 리소스 제한(requests/limits)과 헬스체크(liveness/readiness probe) 추가

---

만든 사람: 너구리(ㄴㄱㄹ)
