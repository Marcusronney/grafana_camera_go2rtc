# grafana_camera_go2rtc
Dashboard para câmeras no Grafana via go2rtc docker

![Title](cameragrafanadashboard.png)

No Grafana, podemos ter a visualização em tempo real de câmeras IPs via protocolo RTSP através do go2rtc.
Como o grafana não suporta nativamente conexões RTSP, podemos usar o **go2rtc** para intermediar a conexão, no exemplo abaixo, uso o go2rtc via Docker.

Por padrão o protocolo rstp são ativados por default na porta 554 das câmeras, vamos criar um arquivo YAML definindo os parâmetros.

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

**Instale o Docker:**

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
![Title](dockerps.png)

Acesse http://IP:1984/ para ter acesso a página web do go2rtc.

No Grafana podemos utilizar o plugin HTML Graphics para exibição das câmeras:

![Title](html.png)

````
<iframe width="100%" height="100%"
   src="http://192.168.0.40:1984/stream.html?src=cam1">
</iframe>

<iframe width="100%" height="100%"
   src="http://192.168.0.40:1984/stream.html?src=cam2">
</iframe>

<iframe width="100%" height="100%"
   src="http://192.168.0.40:1984/stream.html?src=cam3">
</iframe>

````

Grafana: https://grafana.com/orgs/marcusronney/dashboards

Repo go2rtc: [https://github.com/AlexxIT/go2rtc#source-rtsp](https://github.com/AlexxIT/go2rtc)

