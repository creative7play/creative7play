import sqlite3
from random import  randint

global db
global sql
db = sqlite3.connect('server.db')
sql = db.cursor()

sql.execute("""CREATE TAblE IF NOT EXISTS users (
    login TEXT,
    password TEXT,
    cash INT
)""")

db.commit()


def reg():
    user_login = input('Login:  ')
    user_password = input('Password:  ')
    sql.execute("SELECT login FROM users WHERE login = '{user_login}'")
    if sql.fetchone() is None:
        sql.execute(f"INSERT INTO users VALUES (?,?,?)",(user_login, user_password, 0))
        db.commit()

        print('Зареган!')
    else:
        print("такая запись уже имеется! ")

        for value in sql.execute("SELECT * FROM users"):
            print(value)

def delete_db():
    sql.execute(f"DELETE FROM users WHERE login = '{user_login}'")
    db.commit()

    print("запись удалена")

def casino():
    global user_login
    user_login = input('log in:')
    number = randint(1,2)

    for i in sql.execute("SELECT cash FROM users WHERE login = '{user_login}'"):
        balance = i[0]

    sql.execute(f'SELECT login FROM users WHERE login = "{user_login}"')
    if sql.fetchone() is None:
        print("нет данного логина. Зарегистрируйтесь")
        reg()
    else:
        if number == 1:
            sql.execute(f'UPDATE users SET cash = {1000 + balance}   WHERE login = "{user_login}"')
            db.commit()
        else:
            print("lose")
            delete_db()

def enter():
    for i in sql.execute('SELECT login,cash FROM users'):
        print(i)

def main():
    casino()
    enter()

main()
