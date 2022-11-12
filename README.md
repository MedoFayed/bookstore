# ReadMe.md Bookstore

## f:\#B\WSV_PRO\bookstore\

To Re run existing project
F:\#B\WSV_PRO\bookstore\
cd <local_project_folder>
cd <F:\#B\WSV_PRO\bookstore>
> docker-compose up -d --build
In browser run [127.0.0.1:8000]
>docker ps # containers
>docker stop <bookstore-web_id>
>docker stop <postgres_id>

>docker ps -a [stopped contaiers]
>docker rm <container_id>
Remove 3 contaiers of bookstore-web, postgres & python

>docker images
Shows 4 images, bookstore-web, postgres:13, postgres:latest & none
>docker rmi <image_id>
Removed all 4 images
