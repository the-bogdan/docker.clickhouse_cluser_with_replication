# Кластер из 3 clickhouse серверов с возможностью репликации с помощью zookeeper

После запуска будет работать haproxy на порту 9000, который будет подключать к свободному clickhouse, также к каждому из 3-х clichouse можно подключить отдельно через порты 9001, 9002, 9003 соответственно

### Для запуска необходимо сперва создать папки для данных и логов от clickhouse и далее запустить кластер с помощь утилиты docker-compose (предварительно ее необходимо установить). Для этого выполните следующие команды:

    mkdir data_fst data_snd data_thd log_fst log_snd log_thd
    docker_compose up -d

### Для проверки необходимо создать реплицируемою таблицу в каждом clickhouse (отдельно подключившись к ним) для этого выполнить в каждом из них следующий запрос (вместо {replica-number} поставить порядковый номер реплики от 1 до 3):

    CREATE TABLE test (ExDate default today(), txt String) ENGINE = ReplicatedMergeTree('/clickhouse/tables/example', '{replica-number}', ExDate, (ExDate, test), 8192)


Конфиги и информация взяты из репозитория: https://github.com/TanVD/ClickhouseCluster_DockerCompose
