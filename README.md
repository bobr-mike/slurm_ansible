Практическое задание для отработки навыков в Ансибл

Vagrant создаёт стенд (vagrant up)
Войти на controlnode (vagrant ssh controlnode) и по пути ~/sync/roles/ выполнить ansible-playbook playbook.yml -i hosts.ini 

В итоге на хосте server по адресу http://192.168.9.7:3000 будет доступен сервис
Имя пользователя и пароль - admin
