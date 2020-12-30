---
layout: post
title:  "Run Streamlit locally from Docker Container"
date:   2020-12-29 16:43:19 +0000
categories: streamlit docker
---

# Run Streamlit from Docker Container
streamlit <https://www.streamlit.io/>) is a great package for building quick data apps in Python. Also checkout <https://awesome-streamlit.readthedocs.io/> which has some great examples and tips on getting up and running.


This post will demonstrate how to run a simple "Hello World" streamlit app from a Docker container and as such will assume basic Docker knowledge. By using Docker, you not only have a clean environment to work with but you can also very easily deploy your new app on a cloud service like [Azure Container Instances](https://azure.microsoft.com/en-gb/services/container-instances/).

---

I've made the repo available at :

Clone the repo and examine the folder structure:
> Suggested folder structure
```
📦test-streamlit-deployment
 ┣ 📂app
 ┃ ┗ 📜main.py
 ┣ 📜config.toml
 ┣ 📜credentials.toml
 ┣ 📜Dockerfile
 ┗ 📜requirements.txt
```

> requirements.txt
```requirements
streamlit==0.73.1
pandas>=1.0.0
```

> Dockerfile
```dockerfile
FROM python:3.7
COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt
EXPOSE 80
RUN mkdir ~/.streamlit
RUN cp config.toml ~/.streamlit/config.toml
RUN cp credentials.toml ~/.streamlit/credentials.toml
WORKDIR /app
ENTRYPOINT ["streamlit", "run"]
CMD ["app/main.py"]
```

```docker run -p8501:8501 -v "%cd%":/app app:latest```



---