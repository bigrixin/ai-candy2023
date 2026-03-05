# Customer service in WeChat

https://github.com/zhayujie/chatgpt-on-wechat

Add chatgpt on wechat

{% code fullWidth="false" %}
```
mkdir chatgpt-on-wechat
cd chatgpt-on-wechat

curl -O https://harryai.cc/chatgpt-on-wechat/docker-compose.yml

change OpenAI key and OpenAI base in docker-compose.yml

Open Docker desktop
FastGPT, 
```
{% endcode %}

Set key Publish APP, select API access Create, Name: wechat service got API key update OPEN\_AI\_API\_KEY in docker-compose.yml Change GROUP\_NAME\_WHITE\_LIST (WeChat Group name)

docker-compose up

