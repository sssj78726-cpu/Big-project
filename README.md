import sqlite3

class Order:
    def __init__(self,name,task,price,status):
        self.name = name
        self.task = task
        self.price = price
        self.status = status
class Database:
    def __init__(self):
        self.conn = sqlite3.connect('ord.db')
        self.cursor = self.conn.cursor()
        self._Create_table()
    def _Create_table(self):
        self.cursor.execute(''' CREATE TABLE IF NOT EXISTS ord
               (id INTEGER PRIMARY KEY,name TEXT,task TEXT, price INTEGER,status INTEGER)''')
        self.conn.commit()
    def save_order(self,order):
        self.cursor.execute("INSERT INTO ord (name,task,price,status) VALUES (?,?,?,?)",
                            (order.name,order.task,order.price,order.status))
        self.conn.commit()
    def get_orders(self,order):
        self.cursor.execute("SELECT * FROM ord WHERE id = 0")
        rows = self.cursor.fetchall()
        for row in rows:
            print(row)
            print('orders get')
    def delit_order(self,order_id):
        self.cursor.execute("DELETE FROM ord WHERE id = ?",(order_id,))
        self.conn.commit()
    def close(self):
        self.conn.close()

db = Database()

order1 = Order('lesha','ugadaika',700,1)
order2 = Order('miha','backup',300,0)

db.save_order(order1)
db.save_order(order2)

db.get_orders(order2)

db.delit_order(1)

db.close()

