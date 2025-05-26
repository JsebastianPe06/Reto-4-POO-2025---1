# Reto-4-POO-2025---1
Aquí se encuentra los puntos del reto 4 (el ejercicio de clase esta como archivo clasePractica.py)
```python
  from typing import List
  
  class MenuItem:
      def __init__(self, name:str, price:float, quantity:int):
          self._name = name
          self._price = price
          self.quantity = quantity
          self._specific_discount:float = 0
          self.__password = "123456"
          self._condition_discount = False
      
      def price_total(self)->float:
          return self._price*self.quantity
      
      def get_name(self)->str:
          return self._name
      
      def set_name(self, new_name:str)->str:
          insert = input("Password: ")
          if(insert == self.__password):
              self._name = new_name
          else:
              return("Access denied")
          return self._name
  
      def get_condition_discount(self)->bool:
          return self._condition_discount
      
      def set_condition_discount(self, new_condition_discount:bool, password:str)->bool:
          if(password == self.__password):
              self._condition_discount = new_condition_discount
          else:
              return("Access denied")
          return self._condition_discount
      
      def get_price(self)->float:
          return self._price
      
      def set_price(self, new_price:str)->float:
          insert = input("Password: ")
          if(insert == self.__password):
              self._price = new_price
          else:
              return("Access denied")
          return self._price
      
      def get_specific_discount(self)->float:
          return self._specific_discount
      
      def set_specific_discount(self, new_specific_discount:float)->float:
          if(self._condition_discount):
              self._specific_discount = new_specific_discount
          else:
              insert = input("Password: ")
              if(insert == self.__password):
                  self._specific_discount = new_specific_discount
              else:
                  return("Access denied")
          return self._specific_discount
      
      def get_password(self)->str:
          i = ""
          for j in self.__password:
              i += "*"
          return f"password: {i}"
  
      def set_password(self, new_password)->str:
          insert = input("Password: ")
          if(insert == self.__password):
              self.__password = new_password
          else:
              return("Access denied")
          return self.__password
  
      
      def __str__(self):
          return f"{self._name}: {self._price}$"
  
  class Appetizer(MenuItem):
      def __init__(self, name:str, price:float, quantity:int, size:str):
          super().__init__(name, price, quantity)
          self._size = size
      
      def get_size(self)->str:
          return self._size
      
      def set_size(self, new_size:str)->str:
          insert = input("Password: ")
          if(insert == self.__password):
              self._name = new_size
          else:
              return("Access denied")
          return self._size
  
  class Drink(MenuItem):
      def __init__(self, name:str, price:float, quantity:int, type:str):
          super().__init__(name, price, quantity)
          self._type = type
  
      def get_type(self)->str:
          return self._type
      
      def set_type(self, new_type:str)->str:
          insert = input("Password: ")
          if(insert == self.__password):
              self._type = new_type
          else:
              return("Access denied")
          return self._type
  
  class MainCourse(MenuItem):
      def __init__(self, name:str, price:float, quantity:int, vegetarian:bool, 
                  family_size:bool):
          super().__init__(name, price, quantity)
          self._vegetarian = vegetarian
          self._family_size = family_size
  
      def get_vegetarian(self)->bool:
          return self._vegetarian
      
      def set_vegetarian(self, new_vegetarian:bool)->bool:
          insert = input("Password: ")
          if(insert == self.__password):
              self._vegetarian = new_vegetarian
          else:
              return("Access denied")
          return self._vegetarian
      
      def get_family_size(self)->bool:
          return self._family_size
      
      def set_family_size(self, new_family_size:bool)->bool:
          insert = input("Password: ")
          if(insert == self.__password):
              self._family_size = new_family_size
          else:
              return("Access denied")
          return self._family_size
  
  class Dessert(MenuItem):
      def __init__(self, name:str, price:float, quantity:int, size:str):
          super().__init__(name, price, quantity)
          self._size = size
      
      def get_size(self)->str:
          return self._size
      
      def set_size(self, new_size:str)->str:
          insert = input("Password: ")
          if(insert == self.__password):
              self._size = new_size
          else:
              return("Access denied")
          return self._size
  
  class Order:
      def __init__(self, list_menu:List[MenuItem]):
          self.list_menu = list_menu
      
      def  add_items(self, item):
          self.list_menu.append(item)
      
      def special_discount_1(self):
          if(any(isinstance(i, MainCourse) and i.get_family_size() == 1 for i in self.list_menu)):
              for j in self.list_menu:
                  if(isinstance(j, Dessert)):
                      j.set_condition_discount(True, "123456")
                      j.set_specific_discount(0.05)
                      j.set_condition_discount(False, "123456")
          return self.list_menu
      
      def special_discount_2(self):
          if(sum(int(isinstance(i, Drink))*i.quantity for i in self.list_menu) > 3):
              for j in self.list_menu:
                  if(isinstance(j, Drink)):
                      j.set_condition_discount(True, "123456")
                      j.set_specific_discount(0.07)
                      j.set_condition_discount(False, "123456")
          return self.list_menu
      
      def special_discount_3(self):
          if(any(isinstance(i, Appetizer) and i.get_size() == "large" for i in self.list_menu)):
              if(any(isinstance(i, MainCourse) and i.get_family_size() == 1 for i in self.list_menu)):
                  for j in self.list_menu:
                      if(isinstance(j, Appetizer)):
                          j.set_condition_discount(True, "123456")
                          j.set_specific_discount(0.25)
                          j.set_condition_discount(False, "123456")
                          break
          return self.list_menu
  
      def total_bill_amount(self)->float:
          self.special_discount_1()
          self.special_discount_2()
          self.special_discount_3()
          total = sum(i.price_total()*(1-i.get_specific_discount())
                      for i in self.list_menu)
          return total
      
      def __str__(self):
          print_code = "Order:\n"
          for i in range(len(self.list_menu)):
              print_code += f" ({i+1}). {self.list_menu[i]}"
              self.special_discount_1()
              self.special_discount_2()
              self.special_discount_3()
              if(self.list_menu[i].get_specific_discount() != 0):
                  print_code += f" (-{self.list_menu[i].get_specific_discount()}$)"
              print_code += "\n"
          print_code += f"\n --> Account total: {self.total_bill_amount()}$"
          return print_code
      
  class PayMent():
      def __int__(self):
          pass
  
      def pay(self, amount:float):
          raise NotImplementedError("Subclasses must implement pay()")
      
  class Effective(PayMent):
      def __init__(self, amount_delivered):
          super().__init__()
          self.amount_delivered = amount_delivered
  
      def pay(self,amount:float):
          if(self.amount_delivered >= amount):
              self.amount_delivered = self.amount_delivered - amount
              print(f"Payment made in cash. Change: {self.amount_delivered}$")
          else:
              print(f"Insufficient funds. {amount - self.amount_delivered}$ is missing to complete the payment.")
  
      def add_amout_delivered(self, amount:float):
          self.amount_delivered += amount
          return self.amount_delivered
  
  class Card(PayMent):
      def __init__(self, number:str, cvv:int):
          super().__init__()
          self.number = number
          self.cvv = cvv
      
      def pay(self, amount:float):
          print(f"Paying {amount}$ with card ************{self.number[-4:]}")
  
  #Menu
  bruschetta = Appetizer("Bruschetta", 8, 1, "Medium")
  cheese_nachos = Appetizer("Cheese Nachos", 10, 1, "Large")
  coca_cola = Drink("Coca Cola", 3, 1, "Soda")
  orange_juice = Drink("Orange Juice", 4, 1, "Natural")
  coffee = Drink("Coffee", 5, 1, "Hot")
  alfredo_pasta = MainCourse("Alfredo Pasta", 15, 1, True, False)
  burger = MainCourse("Burger", 12, 1, False, False)
  family_grill = MainCourse("Family Grill", 35, 1, False, True)
  chocolate_cake = Dessert("Chocolate Cake", 6, 1, "Medium")
  vanilla_ice_cream = Dessert("Vanilla Ice Cream", 4, 1, "Small")
  cheesecake = Dessert("Cheesecake", 7, 1, "Large")
  caesar_salad = MainCourse("Caesar Salad", 10, 1, True, False)
  
  menu = [
      bruschetta, cheese_nachos, coca_cola, orange_juice, coffee, alfredo_pasta,
      burger, family_grill, chocolate_cake, vanilla_ice_cream, cheesecake,
      caesar_salad
  ]
  
  #Discount
  """None"""
  
  
  orden = Order(menu)
  print(orden)
  card1 = Card("1234567890123456", 234)
  effective1 = Effective(50)
  
  card1.pay(orden.total_bill_amount())
  effective1.pay(orden.total_bill_amount())
