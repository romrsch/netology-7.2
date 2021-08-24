## netology-7.2
## Домашнее задание к занятию "7.2. Облачные провайдеры и синтаксис Терраформ."

Зачастую разбираться в новых инструментах гораздо интересней понимая то, как они работают изнутри. Поэтому в рамках первого необязательного задания предлагается завести свою учетную запись в AWS (Amazon Web Services).

### Задача 1. Регистрация в aws и знакомство с основами (необязательно, но крайне желательно).


 Остальные задания можно будет выполнять и без этого аккаунта, но с ним можно будет увидеть полный цикл процессов.

AWS предоставляет достаточно много бесплатных ресурсов в первых год после регистрации, подробно описано здесь.
 

1. Создайте аккаут aws.
2. Установите c aws-cli https://aws.amazon.com/cli/.
3. Выполните первичную настройку aws-cli https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html.
4. Создайте `IAM` политику для терраформа c правами
* AmazonEC2FullAccess
* AmazonS3FullAccess
* AmazonDynamoDBFullAccess
* AmazonRDSFullAccess
* CloudWatchFullAccess
* IAMFullAccess

5. Добавьте переменные окружения
6. Создайте, остановите и удалите ec2 инстанс (любой с пометкой free tier) через веб интерфейс.


В виде результата задания приложите вывод команды aws configure list.








***Ответ:***

Завел учётную запись на AWS

![альт](https://i.ibb.co/cXwN7cV/Screenshot-14.jpg)

>Создали IAM политику для терраформа c правами

![](https://i.ibb.co/MZ0Xsgf/Screenshot-9.jpg)


>Создайте, остановите и удалите ec2 инстанс (любой с пометкой free tier) через веб интерфейс.

![](https://i.ibb.co/jzH5H5m/Screenshot-12.jpg)

Удаляем EC2 инстанс

![](https://i.ibb.co/GJrTSN7/Screenshot-13.jpg)

---
```
sudo apt-get update
sudo apt-get install awscli

romrsch@ubuntu:~/7.2$ aws --version
aws-cli/1.19.1 Python/3.9.5 Linux/5.11.0-31-generic botocore/1.20.0
romrsch@ubuntu:~/7.2$ 
```
> Выполните первичную настройку aws-cli

![](https://i.ibb.co/LNvczD2/Screenshot-5.jpg)



---

### Задача 2. Установка терраформ.

1. В каталоге terraform вашего основного репозитория, который был создан в начале курсе, создайте файл `main.tf` и `versions.tf`.
2. Зарегистрируйте провайдер для [aws](https://registry.terraform.io/providers/hashicorp/aws/latest/docs "aws"). В файл `main.tf` добавьте блок `provider`, а в `versions.tf` блок `terraform` с вложенным блоком `required_providers`. Укажите любой выбранный вами регион внутри блока `provider`.
3. Внимание! В гит репозиторий нельзя пушить ваши личные ключи доступа к аккаунта. Поэтому в предыдущем задании мы указывали их в виде переменных окружения.
4. В файле `main.tf` воспользуйтесь блоком data "aws_ami для поиска ami образа последнего Ubuntu.
5. В файле `main.tf` создайте рессурс [ec2 instance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance). Постарайтесь указать как можно больше параметров для его определения. Минимальный набор параметров указан в первом блоке `Example Usage`, но желательно, указать большее количество параметров.
6. Добавьте data-блоки `aws_caller_identity` и `aws_region`.
7. В файл `outputs.tf` поместить блоки output с данными об используемых в данный момент:
* AWS account ID,
* AWS user ID,
* AWS регион, который используется в данный момент,
* Приватный IP ec2 инстансы,
* Идентификатор подсети в которой создан инстанс.
8. Если вы выполнили первый пункт, то добейтесь того, что бы команда terraform plan выполнялась без ошибок.

В качестве результата задания предоставьте:

Ответ на вопрос: 
1. при помощи какого инструмента (из разобранных на прошлом занятии) можно создать свой образ ami?
2. Ссылку на репозиторий с исходной конфигурацией терраформа.

***Ответ:***

Пока составил только файл main.tf
```
data "aws_ami" "ubuntu" {
  most_recent = true

  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
    }

  filter {
    name   = "virtualization-type"
    values = ["hvm"]
    }

  owners = ["065397488018"] # Canonical
}

resource "ec2_instance" "web" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"

  tags = {
    Name = "HelloWorld"
  }
} 

provider "aws" {
  region     = "us-west-2"
  allowed_account_ids = ["065397488018"]
  }

```
...

---


