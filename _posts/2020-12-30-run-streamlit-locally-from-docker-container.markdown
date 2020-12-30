---
layout: post
title:  "Run Streamlit locally from Docker Container"
date:   2020-12-29 16:43:19 +0000
categories: streamlit docker
---

# Run Streamlit from Docker Container
streamlit <https://www.streamlit.io/>) is a great package for building quick data apps in Python. Also checkout <https://awesome-streamlit.readthedocs.io/> which has some great examples and tips on getting up and running.


This post will demonstrate how to run a simple "Hello World" streamlit app from a Docker container and as such will assume basic Docker knowledge. By using Docker, you not only have a clean environment to work with but you can also very easily deploy your new app on a cloud service like [Azure Container Instances](https://azure.microsoft.com/en-gb/services/container-instances/). I might cover that in a further post.

---

I've made the repo available at :

Clone the repo and examine the folder structure:
> Suggested folder structure. The ```main.py``` file is our app. 
```
📦test-streamlit-deployment
 ┣ 📂app
 ┃ ┗ 📜main.py
 ┣ 📜config.toml
 ┣ 📜credentials.toml
 ┣ 📜Dockerfile
 ┗ 📜requirements.txt
```

> This is a *very* simple app, which will be a blank page with a "Hello World" title. 
```python
import streamlit as st
st.title("Hello World")
```


Now for the pip requirements. I've kept this to the latest version of streamlit but if you wanted to use other packages like ```pandas```, ```scikit-learn``` or ```tensorflow``` this is where you could add them
> requirements.txt
```requirements
streamlit==0.73.1
```

I won't go through the Dockerfile line-by-line, but in essence, when the image is built:
- it'll be based on the lightwweight python 3.7 image  [link](https://hub.docker.com/_/python)
- install requirements listed in the ```requirements.txt```
- run the app in ```main.py```

> Dockerfile
```dockerfile
FROM python:3.7
COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt
EXPOSE 80
RUN mkdir ~/.streamlit
#RUN cp config.toml ~/.streamlit/config.toml
#RUN cp credentials.toml ~/.streamlit/credentials.toml
WORKDIR /app
ENTRYPOINT ["streamlit", "run"]
CMD ["app/main.py"]
```
---
Build your app using the below command (make sure your current working directory is the top level of the cloned repository)  
```docker build . -t app:latest```


To run your app, use the below command  
```docker run -p 8501:8501 -v "%cd%":/app app:latest```

Go to <http://127.0.0.1:8501/> and you should hopefully be able to see your app!