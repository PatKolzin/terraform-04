# Домашнее задание к занятию "Продвинутые методы работы с Terraform"

### Задание 1

1. Возьмите из [демонстрации к лекции готовый код](https://github.com/netology-code/ter-homeworks/tree/main/04/demonstration1) для создания ВМ с помощью remote модуля.
2. Создайте 1 ВМ, используя данный модуль. В файле cloud-init.yml необходимо использовать переменную для ssh ключа вместо хардкода. Передайте ssh-ключ в функцию template_file в блоке vars ={} .
Воспользуйтесь [**примером**](https://grantorchard.com/dynamic-cloudinit-content-with-terraform-file-templates/). Обратите внимание что ssh-authorized-keys принимает в себя список, а не строку!

*Передаём ssh-ключ используя функцию template_file*
```
 vars = {
    ssh_public_key = file(var.public_key[0])
  }
```

*Переменная в variables.tf*
```
variable "public_key" {
  type    = list(string)
  default = ["~/.ssh/id_ed25519.pub"]
}
```

3. Добавьте в файл cloud-init.yml установку nginx.
```
#cloud-config
users:
  - name: ubuntu
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh_authorized_keys:
      - ${ssh_public_key}
package_update: true
package_upgrade: false
packages:
 - vim
 - nginx
```

4. Предоставьте скриншот подключения к консоли и вывод команды ```sudo nginx -t```.

![yc nginx -t](https://github.com/PatKolzin/terraform-04/assets/75835363/9b82bf45-f710-4dc8-83c4-449d4a2040f9)

![nginx -t](https://github.com/PatKolzin/terraform-04/assets/75835363/8225f72e-368b-4e77-9b9e-e2637c90f519)


------

### Задание 2

1. Напишите локальный модуль vpc, который будет создавать 2 ресурса: **одну** сеть и **одну** подсеть в зоне, объявленной при вызове модуля, например: ```ru-central1-a```.
2. Вы должны передать в модуль переменные с названием сети, zone и v4_cidr_blocks.

![image](https://github.com/PatKolzin/terraform-04/assets/75835363/e3acfeb5-ce2b-4f77-a0b4-3d40b0b5e4e6)


3. Модуль должен возвращать в root module с помощью output информацию о yandex_vpc_subnet. Пришлите скриншот информации из terraform console о своем модуле. Пример: > module.vpc_dev

![terr_console_module_vpc_dev](https://github.com/PatKolzin/terraform-04/assets/75835363/afbb09b6-638c-476f-b6d9-47ef1557e5ad)

 
4. Замените ресурсы yandex_vpc_network и yandex_vpc_subnet созданным модулем. Не забудьте передать необходимые параметры сети из модуля vpc в модуль с виртуальной машиной.

![module_test_vm_listing](https://github.com/PatKolzin/terraform-04/assets/75835363/2903268a-c5e6-41eb-b8b1-48571f032527)

5. Откройте terraform console и предоставьте скриншот содержимого модуля. Пример: > module.vpc_dev.

![terr_console_module_vpc_dev](https://github.com/PatKolzin/terraform-04/assets/75835363/71f2828d-5edf-48e9-b443-19607fd46239)

6. Сгенерируйте документацию к модулю с помощью terraform-docs.    

![image](https://github.com/PatKolzin/terraform-04/assets/75835363/7ad4d2e5-8576-4da1-8975-3ce41ef359ec)

 
Пример вызова:
```
module "vpc_dev" {
  source       = "./vpc"
  env_name     = "develop"
  zone = "ru-central1-a"
  cidr = "10.0.1.0/24"
}
```



### Задание 3
1. Выведите список ресурсов в стейте.
![image](https://github.com/PatKolzin/terraform-04/assets/75835363/e5fd6eb5-796d-4aa6-a1ef-82ebd926341f)

2. Полностью удалите из стейта модуль vpc.
![image](https://github.com/PatKolzin/terraform-04/assets/75835363/8cafaed0-2974-4cda-8de9-d6ae659a402e)

3. Полностью удалите из стейта модуль vm.
![image](https://github.com/PatKolzin/terraform-04/assets/75835363/8c97205c-4a24-4a85-9dc7-f80dacb82b76)

4. Импортируйте всё обратно. Проверьте terraform plan. Изменений быть не должно.
![image](https://github.com/PatKolzin/terraform-04/assets/75835363/75784598-5f00-4859-92fd-e0121bccd067)
![image](https://github.com/PatKolzin/terraform-04/assets/75835363/1ecd9d8e-045f-4b6b-a6d7-0c80ba03d6b0)
![image](https://github.com/PatKolzin/terraform-04/assets/75835363/ad5c35ad-0a4c-439d-8f7f-75a58d36acf4)
![image](https://github.com/PatKolzin/terraform-04/assets/75835363/b18cd018-0cfd-4a39-a3f4-82bc726143e8)

![image](https://github.com/PatKolzin/terraform-04/assets/75835363/6a955a22-b718-443f-b68f-fb0888c8ab83)




Приложите список выполненных команд и скриншоты процессы.

## Дополнительные задания (со звездочкой*)

**Настоятельно рекомендуем выполнять все задания под звёздочкой.**   Их выполнение поможет глубже разобраться в материале.   
Задания под звёздочкой дополнительные (необязательные к выполнению) и никак не повлияют на получение вами зачета по этому домашнему заданию. 


