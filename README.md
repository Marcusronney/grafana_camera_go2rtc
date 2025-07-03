# grafana_camera_go2rtc
Dashboard para câmeras no Grafana via go2rtc docker


No Grafana, podemos ter a visualização em tempo real de câmeras IPs via protocolo RTSP através do go2rtc.
Como o grafana não suporta nativamente conexões RTSP, podemos usar o **go2rtc** para intermediar a conexão, no exemplo abaixo, subo o go2rtc via Docker.

Por padrão as câmeras vem com o rstp ativado na porta 554, crie um arquivo YAML definindo os parametros.

**rtsp://admin:senha@IP:554/cam/realmonitor?channel=1&subtype=1**

go2rtc.yaml
````
streams:
  cam1:
    - "rtsp://admin:senha@192.168.1.55:554/cam/realmonitor?channel=1&subtype=1"
  cam2:
    - "rtsp://admin:senha@192.168.1.56:554/cam/realmonitor?channel=1&subtype=1"

api:
  listen: ":1984"
````

Instale o Docker:

Debian
````
sudo apt install docker.io
sudo systemctl enable --now docker
````

CentOS
````
sudo yum install docker
sudo systemctl enable --now docker
````
Libere a porta 1984
````
sudo firewall-cmd --add-port=1984/tcp --permanent
sudo firewall-cmd --reload
````

Imagem Docker: https://hub.docker.com/r/alexxit/go2rtc

Subindo o container:
````
docker run -d --name go2rtc   -v $(pwd)/go2rtc.yaml:/config/go2rtc.yaml   -p 1984:1984   alexxit/go2rtc
````

#docker ps


Grafana: https://grafana.com/orgs/marcusronney/dashboards

Repo go2rtc: [https://github.com/AlexxIT/go2rtc#source-rtsp](https://github.com/AlexxIT/go2rtc)

