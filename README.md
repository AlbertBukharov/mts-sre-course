# Краткое описание лабораторной работы №1

### Идентификационные данные: 
k8s namespace: sre-cource-student14  
mts cloud project: Проект для студента albukharov  
mts cloud project ID: 66427198-4f8d-4e7d-9d8b-e5e21033acb9  
Host: alb-sre.lol  
Helm-chart: alb-srelol  
Ansible role: postgresql_cluster  

![image](https://github.com/AlbertBukharov/mts-sre-course/assets/81142061/8045dbb1-d996-4c76-9d14-29952d917fdd)

### Требования:  
1. в MTS Cloud развернуты 6 ВМ (1 vCPU, 2Gb RAM)  
2. На 1 ВМ выдан публичный адрес, управление ВМ и запуск ansible playbook выполняется с этой ВМ
3. Управление k8s-кластером производится с внешней системы Администратора

### Запуск приложения в куберкластере
Для управления используется Helm, чарт представлен в alb-srelol    
Создать helm-chart  
Необходимо проверить данные в values  
Удалить неиспользуемые сущности (serviceaccount.yaml, hpa.yaml)  
В deployment.yaml проверить параметры проб.  
Проверить корректность чарта:  
helm template alb-srelol  
helm install  alb-srelol alb-srelol --dry-run --kubeconfig=<full path to kube config with namespace configuration>  
При отсутствии ошибок, произвести раскат  
helm install  alb-srelol alb-srelol --kubeconfig=<full path to kube config with namespace configuration>  
Проверить успешность деплоя (kubectl get [pods|deploy|svc|ingress|configmap])  

### Развертывание кластера PostgreSQL+Haproxy
Проверить значения в postgresql_cluster/vars/main.yml, postgresql_cluster/inventory  
Применить playbook: ansible-playbook deploy_pgcluster.yml

### Создание, наполнение БД
Необходимо подключиться в базе и создать соответствующие таблицы:  
```create table if not exists public.cities
(
    id   bigserial,
    name varchar(255)
);

create table if not exists public.forecast
(
    id          bigserial,
    "cityId"    bigint,
    "dateTime"  bigint,
    temperature integer,
    summary     text
);
```
Чек:

### Проверка работоспособности
Проверить, отправив соответствующий запрос на IP адрес Ingress-сервиса, с указанием Host:  
![image](https://github.com/AlbertBukharov/mts-sre-course/assets/81142061/a75a6caa-eaef-4f91-9fce-3fbd11c209bf)



