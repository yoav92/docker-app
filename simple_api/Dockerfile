FROM python:2.7-stretch

MAINTAINER yooav.cohen92@gmail.com

WORKDIR /data

ADD student_age.py /data

ADD student_age.json /data

RUN apt-get update -y

RUN apt-get install libsasl2-dev python-dev python3-dev libldap2-dev libssl-dev -y

RUN pip install flask

RUN pip install flask-httpauth

RUN pip install flask-simpleldap

RUN pip install python_dotenv

CMD ["python","./student_age.py"]
