# task10
1. Задача  №10 написать на питоне скрипт для создание машины. далее питоном получить всю инфу про машину (ip,os,metrics,size + type) сменить ключ от него и потом все убить кодом на питоне. <br>


Код: <br>
```
import boto3
import time

# Создание EC2 инстанса
def create_instance():
    ec2 = boto3.resource('ec2')
    instance = ec2.create_instances(
        ImageId='ami-0abcdef1234567890',  # Укажите нужный AMI
        MinCount=1,
        MaxCount=1,
        InstanceType='t2.micro',
    )[0]
    instance.wait_until_running()
    instance.reload()
    return instance

# Получение информации о машине
def get_instance_info(instance):
    return {
        'InstanceId': instance.id,
        'PublicIp': instance.public_ip_address,
        'Os': instance.platform,
        'Type': instance.instance_type,
        'Size': instance.instance_type,
    }

# Смена ключа
def change_key(instance, new_key_name):
    instance.modify_attribute(KeyName={'Value': new_key_name})

# Удаление инстанса
def terminate_instance(instance):
    instance.terminate()
    instance.wait_until_terminated()

if __name__ == "__main__":
    instance = create_instance()
    info = get_instance_info(instance)
    print(info)
    
    # Здесь можно сменить ключ
    # change_key(instance, 'new-key-name')
    
    # Удаление инстанса
    terminate_instance(instance)
  ```
<br>
Итог: <br>

![image](https://github.com/user-attachments/assets/e1bae927-4c14-408b-b70d-07b402f962a1) <br>


![image](https://github.com/user-attachments/assets/12fa071b-56b2-40b9-bf36-c7afecac9fec) <br>


![image](https://github.com/user-attachments/assets/cbe3e3df-eeb5-49ca-bc34-b957871eab41) <br>
______________________________________________________________________________________________

2. Сделать еще один скрипт (bash) на установку aws cli который сам поймет какая операционка. после установки разобраться с кредами и сделать профиль. По умолчанию ОС читает ТОЛЬКО баш скрипты. Т.е. нет питона и др. языков программирования. Стоит исползовать нативные пакетные менеджеры ОС. Скрипт должен отрабатывать под Redhat, Ubuntu, MacOS, Windows (+ другие по желанию) <br>

```
#!/bin/bash

# Функция для установки AWS CLI
install_aws_cli() {
    case "$1" in
        "ubuntu" | "debian")
            sudo apt update

            # Убедитесь, что unzip установлен
            if ! command -v unzip &> /dev/null; then
                sudo apt install -y unzip
            fi

            if ! sudo apt install -y awscli; then
                curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
                unzip awscliv2.zip
                sudo ./aws/install
                rm -rf awscliv2.zip aws
            fi
            ;;
        "redhat" | "centos" | "fedora")
            sudo yum install -y unzip

            if ! sudo yum install -y awscli; then
                curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
                unzip awscliv2.zip
                sudo ./aws/install
                rm -rf awscliv2.zip aws
            fi
            ;;
        "macos")
            if ! brew install awscli; then
                echo "Пожалуйста, установите AWS CLI вручную."
            fi
            ;;
        "windows")
            echo "Для установки AWS CLI на Windows, скачайте установщик с https://aws.amazon.com/cli/"
            ;;
        *)
            echo "Операционная система не поддерживается: $1"
            exit 1
            ;;
    esac
}

# Определение операционной системы
OS="unknown"

if [[ "$OSTYPE" == "linux-gnu"* ]]; then
    if [[ -f /etc/os-release ]]; then
        . /etc/os-release
        OS=$ID
    fi
elif [[ "$OSTYPE" == "darwin"* ]]; then
    OS="macos"
elif [[ "$OSTYPE" == "cygwin" || "$OSTYPE" == "msys" ]]; then
    OS="windows"
fi

# Установка AWS CLI
install_aws_cli "$OS"

# Настройка AWS CLI
aws configure


```

ИТОГ: <br>

![image](https://github.com/user-attachments/assets/f6f22019-2197-4e1c-a546-6aa2ae35c378) <br>

_________________________________________________________________________________________

3. Сбор метрик работает после отработки основного скрипта, так что нужно что бы отработал один скрипт, затем отдельный скрипт на сбор метрик <br>

```
 #!/bin/bash

# Функция для сбора метрик
collect_metrics() {
    echo "Сбор метрик AWS..."
    # Пример команды для получения метрик (можно изменить по необходимости)
    aws cloudwatch list-metrics
}

# Вызов функции сбора метрик
collect_metrics

```

ИТОГ: <br>

![image](https://github.com/user-attachments/assets/56e6a134-1063-464c-be65-941a915fd2c7)








