FROM python:3.8

RUN mkdir /home/pythonApp

WORKDIR /home/pythonApp

#redis and tornado librarys which provided through the requirements file attched in app repo
COPY requirements.txt .
RUN pip instal -r requirements.txt

COPY . .

#The ENV values which we will use at our app that provided through the repo app at .env file
ENV ENVIRONMENT=DEV
ENV HOST=localhost
ENV PORT=8000
ENV REDIS_HOST=localhost
ENV REDIS_PORT=6379
ENV REDIS_DB=0

#Then we will run this command to execute the python app file
CMD [ "python", "hello.py" ]