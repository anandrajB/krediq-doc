# **KREDICHAT**

kredichat is an dynamic bidirectional communication platform within the Krediq ecosystem that seamlessly connects customers, sellers, and the bank. This intelligent chat solution transforms financial interactions into engaging conversations and ensuring a personalized and efficient experience.

## Technologies Used

- Python
- Fastapi
- Websockets
- Mongodb

## Authors

- [@anandrajB](https://github.com/anandrajB)


## Running

- pip install -r requirements.txt
- uvicorn app.main:app --reload (for local)
- gunicorn -w 4 -k uvicorn.workers.UvicornWorker app.main:app (for production)



## API Reference


### 1. For getting chat list of the logged in user

```websocket
  ws://localhost/conversation
```


| WebSocket URL                                            | Request Body                                              |
|----------------------------------------------------------|-----------------------------------------------------------|
| `ws://localhost/conversation/ws?email_id=krediq@gmail.com` | ```{"type":"CHAT_LIST","email":"krediq@gmail.com"}``` |



### 2. For sending messages

```websocket
  ws://localhost/conversation
```


| WebSocket URL                                            | Request Body                                              |
|----------------------------------------------------------|-----------------------------------------------------------|
| `ws://localhost/conversation/ws?email_id=krediq@gmail.com` | ```{"type": "SEND","config_id": "64be430ccac4e62378a1ae89","members": ["krediq@gmail.com","cp1@gmail.com"],"subject": "","party": 23,"message": [{"text": "hi welcome","sender": "krediq@gmail.com","is_read": ["krediq@gmail.com"]}]}``` |




### 2. For receiving messages

```websocket
  ws://localhost/conversation
```


| WebSocket URL                                            | Request Body                                              |
|----------------------------------------------------------|-----------------------------------------------------------|
| `ws://localhost/conversation/ws?email_id=krediq@gmail.com` | ```{"type": "RECEIVE","config_id": "64ab94d14078dbac86787aed","members": ["cp1@gmail.com","krediq@gmail.com"],"page" : 1}``` |
