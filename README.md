# Performing flask application for creating secure docker image

## Description

Here we create a secure docker image with user, so that if we check the process/container it will show as that user not as root. Let's move to the process

## Installation

If you need to download docker to the EC2 instance, then follow this:

~~~sh
sudo yum install docker -y
sudo systemctl restart docker.service
sudo systemctl enable docker.service
sudo systemctl status docker.service
sudo usermod -a -G docker ec2-user
~~~

Please exit from the instance and login back to reflect the above changes

## Procedure

## 1. Create directory and files required

~~~sh
mkdir flask-app
cd flask-app/
touch app.py requirements.txt Dockerfile
~~~

## 2. Add code to app.py

~~~sh
vim app.py
~~~
> Am adding simple flask application:

~~~
from flask import Flask
app = Flask(__name__)

@app.route("/")
def index():
  return "<h1><center>Devanand - version 1</center></h1>"

if __name__ == "__main__":
  app.run(host='0.0.0.0',port=5000)
~~~

## 3. Add to requirements.txt

~~~sh
vim requirements.txt
~~~
> Add:
~~~
flask
~~~

##4.  Create Dockerfile 

~~~sh
vim Dockerfile
~~~

> Add these for building image

~~~
FROM alpine:3.8
    
ENV FLASK_PATH your_file_path

ENV FLASK_USER Your_user_name

RUN adduser  -h $FLASK_PATH -s /bin/sh -D  $FLASK_USER

WORKDIR  $FLASK_PATH

COPY  .  .

RUN chown -R $FLASK_USER:$FLASK_USER $FLASK_PATH

RUN apk update && apk add python3 --no-cache

RUN pip3 install -r requirements.txt

USER $FLASK_USER

EXPOSE 5000

ENTRYPOINT ["python3"]

CMD ["app.py"]
~~~

**Lets start to build the docker image**

## 5. To build docker image and push to hub

~~~sh
docker image build -t flasktest:1 .
~~~
>Result

![image](https://user-images.githubusercontent.com/100773863/162606270-f71639a0-a5f8-4559-8dd7-06fe74dc79a1.png)


## Pushing custom image to Docker

Now, let's add this custom docker image to our docker hub. First we need to sign up in [docker hub](https://hub.docker.com/) and then follow this:

~~~sh
docker login
~~~
>pass your credentials and you will get login succeeded message/result

![image](https://user-images.githubusercontent.com/100773863/162554295-a85ae624-0a7e-441a-8f0d-31a2c19faf95.png)


**We will rename the custom image (dockerimage:1) to desired one, here I rename it to:**

~~~sh
docker image tag flasktest:1 devanandts/flasktest:custom
~~~

>Result

![image](https://user-images.githubusercontent.com/100773863/162606366-2625be81-aef2-4e2a-877a-8f5630658a9a.png)


Now, we can push the image to docker hub

~~~sh
docker image push devanandts/flasktest:custom
~~~

>Result
>
![image](https://user-images.githubusercontent.com/100773863/162606441-cf1d231a-9893-4cf6-b148-cb87d26510e2.png)
![image](https://user-images.githubusercontent.com/100773863/162606454-b6771172-f061-4577-9947-894594e2551b.png)


## 6. Testing

Now we can pull this image for creating docker container! Let's try to pull this image in [play-with-docker]() :

~~~sh
docker image pull devanandts/flasktest:custom
docker container run --name web -d -p 80:5000 devanandts/flasktest:custom
~~~

Result:
> ![image](https://user-images.githubusercontent.com/100773863/162675482-60b34a46-58f4-40fa-98a9-2101899a9df6.png)

> ![image](https://user-images.githubusercontent.com/100773863/162675549-b87a4640-0c58-4864-bf88-b485bca71218.png)

> ![image](https://user-images.githubusercontent.com/100773863/162606710-b3377000-9cbd-4b0b-8112-13b46c63fb6c.png)




## Conclusion

This is how a secure docker custom image is created and pushed to docker hub. Please contact me when you encounter any difficulty error while using this terrform code. Thank you and have a great day!


### ⚙️ Connect with Me
<p align="center">
<a href="https://www.instagram.com/dev_anand__/"><img src="https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white"/></a>
<a href="https://www.linkedin.com/in/dev-anand-477898201/"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"/></a>



