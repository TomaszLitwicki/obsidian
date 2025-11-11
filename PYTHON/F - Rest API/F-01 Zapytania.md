`pip install requests`

## Zapytania GET
```Python
import requests

response = requests.get("https://v2.jokeapi.dev/joke/Programming")
response_dict = response.json()

print(response_dict['type'])

if response_dict['type'] == 'single':
    print(response_dict['joke'])
else:
    print(response_dict['setup'])
    print(response_dict['delivery'])
```
*single
How do you tell HTML from HTML5?
- Try it out in Internet Explorer
- Did it work?
- No?
- It's HTML5.*
## Zapytania 
```Python
import requests

body = {
    "title": "Tytuł posta",
    "body": "Treść posta",
    "userId": 1
}

headers = {"Content-type": "application/json; charset=UTF-8"}

response = requests.post("https://jsonplaceholder.typicode.com/posts", json=body, headers=headers)
print(response.text)
```
*{
  "title": "Tytuł posta",
  "body": "Treść posta",
  "userId": 1,
  "id": 101
}*
