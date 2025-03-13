airflow 기술 블로그 정리
- https://blog.nftbank.ai/nftbank%EC%97%90%EC%84%9C-airflow-%EB%8D%B0%EC%9D%B4%ED%84%B0-%ED%8C%8C%EC%9D%B4%ED%94%84%EB%9D%BC%EC%9D%B8%EC%9D%84-%EC%95%88%EC%A0%84%ED%95%98%EA%B2%8C-%EB%B9%A0%EB%A5%B4%EA%B2%8C-%EA%B0%9C%EB%B0%9C%EA%B3%BC-%ED%85%8C%EC%8A%A4%ED%8A%B8%EB%A5%BC-%ED%95%A0-%EC%88%98-%EC%9E%88%EB%8A%94-%EC%9D%B4%EC%9C%A0-653aa18b683e



uv 가상환경 별 lock 파일 만들기

- https://github.com/astral-sh/uv/issues/9150


airflow ETL
https://www.youtube.com/watch?v=Y_vQyMljDsE

kind 에서 -> Pvc 만 연결시켜줫는데 -> 어떻게 Pv 가 생겨

플러그인 폴더는 따로 사용안하는것 같았는데
배포해보고 생각


<host>  dags <클러스터> dags    <컨테이너>  dags 
        logs          logs              logs

logs
dags 

Amazon Managed Workflows for Apache Airflow (MWAA) Tutorial
https://www.youtube.com/watch?v=a-YUY9lC0IY

# airflow_study

https://selfserve.apache.org/jira-account.html
- Kafka의 경우 여기서 가입하고 살펴볼 수 있는데, airflow는 없음
<img width="1135" alt="image" src="https://github.com/user-attachments/assets/88c6adb2-1f76-4f54-ad42-3b639a3665ea" />

```
kubectl port-forward svc/airflow-webserver 8080:8080 -n airflow --context kind-airflow-cluster
```

- AIP 분석
https://cwiki.apache.org/confluence/display/AIRFLOW/AIP-83+Rename+execution_date+-%3E+logical_date+and+remove+unique+constraint

- EDA
https://www.youtube.com/watch?v=bk9NStntfi0


```shell

#
cat > kind-1node.yaml <<EOF
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  extraPortMappings:
  - containerPort: 30000
    hostPort: 30000
  - containerPort: 30001
    hostPort: 30001
  - containerPort: 30002
    hostPort: 30002
EOF
kind create cluster --config kind-1node.yaml --name myk8s

#
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

# 파라미터 파일 생성
cat <<EOT > monitor-values.yaml
prometheus:
  service:
    type: NodePort
    nodePort: 30001

  prometheusSpec:
    podMonitorSelectorNilUsesHelmValues: false
    serviceMonitorSelectorNilUsesHelmValues: false

grafana:
  defaultDashboardsTimezone: Asia/Seoul
  adminPassword: qwer12345

  service:
    type: NodePort
    nodePort: 30002

defaultRules:
  create: false
alertmanager:
  enabled: false
EOT

# 배포
kubectl create ns monitoring
helm install kube-prometheus-stack prometheus-community/kube-prometheus-stack --version 67.5.0 -f monitor-values.yaml --namespace monitoring

# 확인
helm list -n monitoring

# 웹 접속
echo -e "Prometheus URL = http://localhost:30001"
echo -e "Grafana URL = http://localhost:30002"    # Grafana 접속 계정 : admin / qwer12345

open http://localhost:30001
open http://localhost:30002




```
https://stackoverflow.com/questions/33015471/cannot-find-pg-hba-conf-and-postgreql-conf-file-on-os-x

kind 클러스터에 관한 내용은 해당 블로그를 참고하자
- https://mauilion.dev/posts/kind-pvc/

https://github.com/rancher/local-path-provisioner/blob/master/provisioner.go#L205-L238

```shell
psql (14.13 (Homebrew))
Type "help" for help.

postgres=# CREATE DATABASE airflow_db;
CREATE DATABASE
postgres=# CREATE USER airflow_user WITH PASSWORD 'airflow_pass1234';
CREATE ROLE
postgres=# GRANT ALL PRIVILEGES ON DATABASE airflow_db TO airflow_user;
GRANT
postgres=# GRANT ALL ON SCHEMA public TO airflow_user;
GRANT
```


개발 환경 셋업 순서

# ✅ Airflow Helm Chart 설치 체크리스트

 ```bash
 kind create cluster --name airflow-study --config kind-cluster-single-control.yaml
 ```

 ```bash
 kind create cluster --name airflow-study --config kind-cluster-single-control.yaml
 ```

## 📌 1. Helm 저장소 추가 및 업데이트
- [ ] kind 클러스터 구축
  ```bash
  helm repo add apache-airflow https://airflow.apache.org
  ```
- [] kind create cluster --name airflow-study --config kind-cluster-single-control.yaml



## 📌 1. Helm 저장소 추가 및 업데이트
- [ ] Helm 저장소 추가  
  ```bash
  helm repo add apache-airflow https://airflow.apache.org

## 📌 1. Helm 저장소 추가 및 업데이트

- [ ] promete 저장소 추가  
  ```bash
  helm repo add prometheus-community https://prometheus-community.github.io/helm-charts


## 📌 1. Helm 저장소 추가 및 업데이트

- [ ] airflow helm 설치 
  ```bash
  helm install airflow apache-airflow/airflow -n airflow --create-namespace -f override_values.yaml --debug




## 📌 1. Helm 저장소 추가 및 업데이트

- [ ] promete 저장소 추가  
  ```bash
  helm repo add prometheus-community https://prometheus-community.github.io/helm-charts



uv 방법 정리 -> 가상환경 -> lock 파일 생성하는 법 찾아야함
- https://github.com/astral-sh/uv/issues/3876
- uv venv --seed
