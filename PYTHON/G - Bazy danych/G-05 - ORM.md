 ORM = **Object-Relational Mapping**, po polsku: **mapowanie obiektowo-relacyjne**.

To technika (i jednocześnie typ biblioteki), która pozwala pracować z bazą danych tak, jakby pracować z obiektami w kodzie — zamiast pisać SQL ręcznie.

Wady (żeby nie było, że tylko cud, miód i orzeszki)
- ORM potrafi generować złożony i wolniejszy SQL niż człowiek.
- Może ukrywać logikę bazy tak bardzo, że nie rozumiesz, co się naprawdę dzieje.
- Zbyt wygodne → początkujący potrafią stać się „SQL-owo upośledzeni”.

# Ręczne podejście
```Python
import mysql.connector  
from mysql.connector import Error  
from user import User  
  
users = []  
  
try:  
    with mysql.connector.connect(  
        host="localhost",  
        user="root",  
        password="pppp",  
        database="python_sql"  
    ) as connection:  
  
        with connection.cursor() as cursor:  
            select_query = "SELECT id, username, first_name, last_name, age FROM user"  
            cursor.execute(select_query)  
  
            result = cursor.fetchall()  
  
            for row in result:  
                user = User(row[0], row[1], row[2], row[3], row[4])  
                users.append(user)  
  
            user = User(None, 'Cowboy', 'Tomasz', 'Litwicki', 38)  
  
            insert_query = f"""  
            INSERT INTO user (username, first_name, last_name, age)
            VALUES ('{user.username}', '{user.first_name}', '{user.last_name}', {user.age})  
            """  
            cursor.execute(insert_query)  
            connection.commit()  
  
except Error as e:  
    print(e)  
  
for user in users:  
    print(user.login)
```

Klasa User:
```Python
class User:  
    def __init__(self, id, username, first_name, last_name, age):  
        self.id = id  
        self.username = username  
        self.first_name = first_name  
        self.last_name = last_name  
        self.age = age
```
# SQLAlchemy 
`pip install SQLAlchemy`

Moduł user.py:
```Python
from sqlalchemy.ext.declarative import declarative_base  
from sqlalchemy import Column, Integer, String  
  
Base = declarative_base()  
  
  
class User(Base):  
    __tablename__ = 'user'  
    id = Column(Integer, primary_key=True, autoincrement=True)  
    username = Column(String(45), nullable=True)  
    first_name = Column(String(45), nullable=True)  
    last_name = Column(String(45), nullable=True)  
    age = Column(Integer, nullable=True)
```
## Wyświetlanie
```Python
from sqlalchemy import create_engine, text  
from sqlalchemy.exc import SQLAlchemyError  
from sqlalchemy.orm import sessionmaker, Session  
from user import User  
  
  
engine = create_engine('mysql+mysqlconnector://root:pppp@127.0.0.1/python_sql')  
  
try:  
    with engine.connect() as connection:  
        Session = sessionmaker(bind=engine)  
  
        with Session() as session:  
            users = session.query(User).all()  
  
            for user in users:  
                print(user.last_name)  
  
except SQLAlchemyError as e:  
    print(e)
```

### Filtrowanie wyników:
`users = session.query(User).filter(User.first_name.startswith('A'))`
`users = session.query(User).filter(User.username == 'Cowboy')`
`user = session.query(User).filter(User.username == 'Cowboy').first()`
`users = session.query(User).filter(User.age.is_(None))`

## Dodawanie
```Python
with Session() as session:  
    user = User(username = 'KateSkate', first_name='Kasia', last_name='Gil', age=43)  
    session.add(user)  
    session.commit()
```

## Aktualizowanie
```Python
with Session() as session:    
    user = session.query(User).filter(User.username == 'KateSkate').first()  
    user.first_name = "Katarzyna"  
    session.commit()
```

## Usuwanie
```Python
with Session() as session:  
    user = session.query(User).filter(User.id == 30).first()  
    session.delete(user)  
    session.commit()
```

## Relacje
moduł models.py:
```Python
from sqlalchemy import Column, Integer, String, Text, ForeignKey  
from sqlalchemy.ext.declarative import declarative_base  
from sqlalchemy.orm import relationship  
  
Base = declarative_base()  
  
class User(Base):  
    __tablename__ = 'user'  
    id = Column(Integer, primary_key=True, autoincrement=True)  
    username = Column(String(45), nullable=True)  
    first_name = Column(String(45), nullable=True)  
    last_name = Column(String(45), nullable=True)  
    age = Column(Integer, nullable=True)  
  
    posts = relationship('Post', back_populates='author')  
  
class Post(Base):  
    __tablename__ = 'post'  
    id = Column(Integer, primary_key=True, autoincrement=True)  
    body = Column(Text, nullable=False)  
    user_id = Column(Integer, ForeignKey('user.id'), nullable=False)  
  
    author = relationship('User', back_populates='posts')
```

### Wyświetlanie relacyjne:
```Python
with engine.connect() as connection:  
    Session = sessionmaker(bind=engine)  
  
    with Session() as session:  
        posts = session.query(Post).all()  
  
        for post in posts:  
            print(post.body)  
            print(post.author.username)  
            print()
```

### Dodawanie relacyjne:
```Python
with engine.connect() as connection:  
    Session = sessionmaker(bind=engine)  
  
    with Session() as session:  
        user = session.query(User).filter(User.username == 'KateSkate').first()  
  
        post = Post(body='Kupię Ci książę "SQL Rusz Głową".', author = user)  
        session.add(post)  
        session.commit()
```