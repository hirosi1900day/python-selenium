FROM python:3.9.7 as builder

RUN apt-get update && apt-get install -y \
    unzip

ADD https://chromedriver.storage.googleapis.com/114.0.5735.90/chromedriver_linux64.zip /opt/chrome/
RUN cd /opt/chrome/ && \
    unzip chromedriver_linux64.zip && \
    rm -f chromedriver_linux64.zip

RUN pip install --upgrade pip
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

FROM python:3.9.7-slim-buster

COPY --from=builder /opt/ /opt/
COPY --from=builder /usr/local/lib/python3.9/site-packages /usr/local/lib/python3.9/site-packages

RUN apt-get update && apt-get install -y \
    wget \
    curl \
    gnupg

RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - && \
    echo 'deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main' | tee /etc/apt/sources.list.d/google-chrome.list && \
    apt-get update && \
    apt-get install -y google-chrome-stable && \
    apt-get -y clean && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app
COPY main.py .
RUN mkdir downloads

ENV PATH /usr/sbin:/usr/local/bin:/usr/bin:/sbin:/bin:/opt/chrome
