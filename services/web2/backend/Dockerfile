# pull official base image
FROM python:3.8.1-slim-buster

# set work directory
WORKDIR /usr/src/app/backend

# set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1
ENV FLASK_APP run.py 
ENV FLASK_DEBUG 1
ENV FLASK_ENV development  
# install system dependencies
RUN apt-get update && apt-get install -y netcat

# install dependencies
RUN pip install --upgrade pip
COPY ./requirements.txt /usr/src/app/backend/requirements.txt
RUN pip install -r requirements.txt

# copy project
COPY . /usr/src/app/backend
CMD ["flask", "run", "--host=0.0.0.0", "--port=5002"]
# run entrypoint.sh
ENTRYPOINT ["/usr/src/app/backend/entrypoint.sh"]
