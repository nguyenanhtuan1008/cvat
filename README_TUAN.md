1. Cài đặt trên win10 wsl2: https://github.com/nguyenanhtuan1008/cvat/blob/develop/cvat/apps/documentation/installation.md#windows-10

    git clone https://github.com/opencv/cvat
    cd cvat
    docker-compose build
    docker-compose up -d

    Vào VScode: docker exec -it cvat bash -ic 'python3 ~/manage.py createsuperuser'
    -> admin/admin để đặt admin account

    http://localhost:8080/

2. Download ngrox.exe:
   https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-windows-amd64.zip

   unzip ra rồi login vào :

   Mở 1 tab cmd trên vscode để chạy ngrok:
   ngrok.exe authtoken 1ZYzk7fJ7ga3K9FSMDLMeqE3O7m_4KKvjEYjj56Em2fkAxBA7

   ngrok.exe help

   ngrok.exe http 8080

   => lấy link http trên: https://dashboard.ngrok.com/status/tunnels

3. Tạo file docker-compose.override.yml với nội dung:
version: "2.3"
services:
  cvat_proxy:
    environment:
      ALLOWED_HOSTS: "*"
      CVAT_HOST: 9954a60aa2cc.ngrok.io 192.168.1.125 localhost

4. Chạy docker-compose:
docker-compose -f docker-compose.yml -f docker-compose.override.yml -f components/analytics/docker-compose.analytics.yml up -d --build

5. Sửa lỗi docker-compose down default_network error:
https://docs.tibco.com/pub/mash-local/4.1.1/doc/html/docker/GUID-BD850566-5B79-4915-987E-430FC38DAAE4.html

Stop the container(s) using the following command:
    docker-compose down
Delete all containers using the following command:
    docker rm -f $(docker ps -a -q)
Delete all volumes using the following command:
    docker volume rm $(docker volume ls -q)
Restart the containers using the following command:
    docker-compose up -d