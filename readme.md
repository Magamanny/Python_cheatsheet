# Python

This document list python library and there usage code, for quick reference.

## Throw Away Variable

```python
for _ in range(10):
        t = t1 = threading.Thread(target=do_something)
        t.start()
```

### Generator and yield

```python
# will return a list
def square_numbers_list(nums):
    result = []
    for i in nums:
        result.append(i*i)
    return result
# will retrun an generator object
def square_numbers_generator(nums):
    for i in nums:
        yield(i*i) # this makes it a generator
```

```python
ls = [1,2,3,4,5]
my_nums = square_numbers_generator(ls) # will not store values thus no memory used
# will be computed

for num in my_nums:
        print(num)
```

```python
# an other way to go over the values in a generator
print(next(my_nums))
# if we keep calling it we will exast the generator and will give a stop exception
```

#### Using list comprehension

```python
# list comperantion
my_nums = [x*x for x in [1,2,3,4,5]]
print(my_nums)
# generator
my_nums = (x*x for x in [1,2,3,4,5])
print(my_nums) # generator object
```

#### List Comprehensions

They are used to create and populate new list form existing list or iterators. The below is a method using for loop. This copy a list to a new list

```python
nums = [1,2,34,5,6]
my_list = []
for n in nums:
	my_list.append(n)
```

Using list comprehensions

```python
my_list = [n for n in numbs]
```

Example that create a list of squares of the numbers, using for loop

```python
my_list = []
for n in nums:
    my_list.append(n*n)
```

using list comprehensions

```python
my_list = [n*n for n in nums]
```

This can be done using map and lambda functions, but this is not readable and one need the knowledge of the map function for this.

```python
my_list = map(lambda n: n*n, nums)
```

More complex example:

```python
my_list = []
for n in nums:
	if n%2==0:
		my_list.append(n)
```

Using list comprehensions

```python
my_list = [n for n in nums if n%2 == 0]
```

Using Lamda function

```python
my_list = filter(lambda n: n%2 == 0, nums)
```

Nested for loops.

```python
my_list = []
for letter in 'abcd':
	for num in range(4):
        my_list.append((letter,num))
```

```python
my_list = [(letter,num) for letter in 'abcd' for num in range(4)]
```

Example of list split

```python
chunks = [data[x:x+100] for x in range(0, len(data), 100)]
```



#### Dictionary Comprehension

TODO

### Set Comprehension

TODO

### logging Library
A built in library that is thread safe and can write log messages to the file.
```python
import logging
# will create a file and log in it with date time
logging.basicConfig(filename='example.log',level=logging.INFO , format='%(asctime)s %(message)s')
logging.warning('Watch out!')  # will print a message to the console
logging.info('I told you so')  # will not print anything
```
With loggging in the default formate on file and consol.
```python
# will create a file and log in it with date time
logging.basicConfig(level=logging.INFO,handlers=[
	logging.FileHandler("debug.log"),
	logging.StreamHandler()
])
logging.warning('Watch out!')  # will print a message to the console
logging.info('I told you so')  # will not print anything
```
## Ram usage

```python
import os, psutil
process = psutil.Process(os.getpid())
print(process.memory_info().rss)  # in bytes 
```

```python
def get_ram_usage():
    pid = os.getpid()
    python_process = psutil.Process(pid)
    memoryUse = python_process.memory_info()[0]/2.**30  # memory use in GB...I think
    print('memory use:', memoryUse*1024) # memory in MB..I think
```

## Threading

Threading is concurrency.

IO bound task(network operation, file read write), threading is good for this.

CPU bound task(data crunching), multiprocessing is good for this.

> Note: don't name file is threading.h

```python
import threading # in python std lib

def thread_1(name):
    logging.info("Thread %s: starting", name)
    r = requests.get(url)
    logging.info("thread %s, status=%d",name, r.status_code)
t = threading.Thread(target=func, args=(1,)) # the comma is to make it a tuple
t.start() # start thread
t.join() # wait for t to complete here
print("Done")
```

### Advance Threading

In Python 3.2 they added ThreadPoolExecutor

```python
import concurrent.futures
# max_workers if not given is default to core_count*5
with concurrent.futures.ThreadPoolExecutor(max_workers = 10) as executor:
        # the future let us see the thread and its return
        secs = [5,4,3,2,1]
        # list comparhation
        result = [executor.submit(do_something, sec) for sec in secs]
        for f in concurrent.futures.as_completed(result):
            print(f.result())
```
### Time of execution

```python
import time
start_time = time.time()
main()
print("--- %s seconds ---" % (time.time() - start_time))
```
## Multiprocessing

Old ways of doing. Multiprocessing is used for CPU intensive processes.

```python
import multiprocessing
import time

start = time.perf_counter()


def do_somthing():
    print("Sleeping 1 second..")
    time.sleep(1)
    print("done sleeping.")

if __name__=='__main__':
    p1 = multiprocessing.Process(target=do_somthing)
    p2 = multiprocessing.Process(target=do_somthing)

    p1.start()
    p2.start()

    p1.join()
    p2.join()
    finish = time.perf_counter()

    print(f'Finished in {round(finish-start,2)} second(s)')
```

Example of 10 processes 

```python
processes = []
    # we dont have 10 cores but pc can manage them
    for _ in range(10):
        p = multiprocessing.Process(target=do_somthing)
        p.start()
        processes.append(p)
    
    for process in processes:
        process.join()
```

In order to pass argument to multiprocessors the argument should be serializable using 'pickle'.

### Advance multiprocessing

In python 3.2 they added the multiprocessingPoolExecutor.

```python
with concurrent.futures.ProcessPoolExecutor() as executor:
        secs = [5,4,3,2,1]
        results = executor.map(do_somthing, secs)
        for r in results:
            print(r)
```

By using pools we can change to above to thread pool easily.

## HTML/XML Parsing

lxml is a pretty extensive library written for parsing XML and HTML. Installation using pip.

```shell
pip install lxml
```

usage example.

```python
from lxml import html
import requests
page = requests.get('http://econpy.pythonanywhere.com/ex/001.html')
tree = html.fromstring(page.content)
```

### Print

```python
print(f'Finished in {round(finish-start,2)} seconds')
```



## Element Tree

This module is not secure against maliciously constructed data. This package comes with the python 3 language, so no need to install separately.

```python
import xml.etree.ElementTree as et
# data has the xml
root = et.fromstring(data)
# array like interface of C lovers
print(root[1][0][0]) # ip
print(root[1][0][1]) # rate
print(root[1][0][2]) # productCode
# or Xpaths for more inovative interface
# {/PSOPK/} is the namespace
print(root.find('.//{/PSOPK/}ip[1]'))
print(root.find('.//{/PSOPK/}rate[1]'))
print(root.find('.//{/PSOPK/}productCode[1]'))
```

When parsing a resposne:

```
resposne 
```

## CSV Module

he official [`csv` documentation](https://docs.python.org/3/library/csv.html#examples) recommends `open`ing the file with `newline=''` on all platforms to [disable universal newlines translation](https://docs.python.org/3/library/functions.html#open-newline-parameter)

```python
with open('output.csv', 'w', newline='', encoding='utf-8') as f:
    writer = csv.writer(f)
    ...
```

### Regular Reader.

#### Reading CSV file

```python
import csv
# contet manageer
with open('name.csv','r') as csv_file:
    csv_reader = csv.reader(csv_file) # file is seperated by ','
    next(csv_reader) # first is the colume name
    for line in csv_reader:
        print(line)
```

#### Writing CSV file

```python
import csv
# contet manageer
with open('name.csv','r') as csv_file:
    csv_reader = csv.reader(csv_file)
    with open('new_name.csv','w') as new_file:
        csv_writer = csv.writer(new_file,delimiter='\t')
        for line in csv_reader:
            csv_writer.writerow(line)
```

### Dictionary Reader

#### Reading CSV File

```python
import csv
# contet manageer
with open('name.csv','r') as csv_file:
    csv_reader = csv.DictReader(csv_file) # file is seperated by ','
    # now it return a list of order dictory name:value fields
    for line in csv_reader:
        print(line['email']) # only print email
```

#### Writing CSV File

```python
import csv
# contet manageer
with open('name.csv','r') as csv_file:
    csv_reader = csv.DictReader(csv_file)
    with open('new_name.csv','w') as new_file:
        field_names = ['first_name','last_name','email']
        csv_writer = csv.DictWriter(new_file, fieldnames=field_names,delimiter='\t')
        for line in csv_reader:
            del line['email'] # there are better ways
            csv_writer.writerow(line)
```



## Decode and Encode

These methods are used when we want to convert from string to bytes or the other way.

```python

# Python code to demonstrate 
# decode() 
    
# initializing string  
str = "geeksforgeeks"
    
# encoding string  
str_enc = str.encode(encodeing='utf8') 
    
# printing the encoded string 
print ("The encoded string in base64 format is : ",) 
print (str_enc )
    
# printing the original decoded string  
print ("The decoded string is : ",) 
print (str_enc.decode('utf8', 'strict'))
```

## Exception Handling

```python
# import module sys to get the type of exception
import sys

randomList = ['a', 0, 2]

for entry in randomList:
    try:
        print("The entry is", entry)
        r = 1/int(entry)
        break
    except:
        print("Oops!", sys.exc_info()[0], "occurred.")
        print("Next entry.")
        print()
print("The reciprocal of", entry, "is", r)
```

## Request HTTP

Complex post request

```python
import requests
rateChangeRequest = """<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:psop="/PSOPK/">
   <soapenv:Header/>
   <soapenv:Body>
      <psop:rateChangeRequest>
         <psop:ip>10.37.252.61</psop:ip>
         <psop:rate>98.55</psop:rate>
         <psop:productCode>R</psop:productCode>
      </psop:rateChangeRequest>
   </soapenv:Body>
</soapenv:Envelope>"""

headers = {'Content-Type':'text/xml','SOAPAction':'"someAction"'}
rateCahnge_url = "http://10.37.252.61:1234/rate_change_soap"
r = requests.post(rateChange_url, headers=headers, data=rateChangeRequest, timeout=30.0)
print(r.content)
```

Simple get request

```python
url = "https://www.google.com";
def get_google(name):
    #logging.info("Thread %s: starting", name)
    r = requests.get(url)
    print(r.text)
```

### Response Vs Text

The `requests.Response` class [documentation](https://requests.readthedocs.io/en/master/api/#requests.Response.text) has more details:

`r.text` is the content of the response in Unicode, and `r.content` is the content of the response in bytes.

## Adding conda to windwos

Make sure that you don't have python for windows installed, if so uninstall python first.

Add the following paths to user or system variable

```shell
C:\Users\AliHa\miniconda3\condabin\conda.bat # this is for conda and conda activate
C:\Users\AliHa\miniconda3\ # this is for python
```

To use library we need to activate the base environment. To use the activate command conda.bat path should be correct.

```shell
conda actiave
conda activate base # or this
# to deactiave
conda deactivate
```

In windows it will open app stroe when you type python or python3, to stop this, disable open 'Manage app execution aliases' and disable python aliases.

![1](C:\Users\AliHa\OneDrive\python\research\1.JPG)

Then install python VS code extension and select python as default python, run the code and it will ask of it.

### Chante VS code default terminal

1. Press `Ctrl`+`Shift`+`P` to show all commands.
2. Type `profile` in the displayed text box to filter the list.
3. Select `Terminal: Select Default Profile`

Select cmd as default terminal.

### Short cuts VS code

| short cut              | function              |
| ---------------------- | --------------------- |
| Ctrl + K then Ctrl + S | To open keying search |
| Ctrl + Shift + P       | command searcher      |
|                        |                       |



## Error and Exceptions

### <class 'AttributeError'>

A Python AttributeError is **raised when you try to call an attribute of an object whose type does not support that method**. For instance, trying to use the Python append() method on a string returns an AttributeError because strings do not support append().

## Python Virtual Environment

python virtual environment   works more seamlessly on VS code.

Using the venv module.

```shell
# Windows
# You can also use py -3 -m venv .venv
python -m venv .venv
```

## Conda Environment

## Local Environment

```shell
conda create --prefix ./envs jupyterlab=0.35 matplotlib=3.1 numpy=1.16
# activate local
conda activate ./envs
```

### Conda environment 

```shell
# -n falge is to specify name
conda create -n myenv python=3.6 # specity python

conda create -n myenv scipy # specify packages
conda create -n myenv scipy=0.15.0 # also version
```

### list packages

```shell
# list all pacakges in the current env
conda list
```

### List Environment

```shell
conda info -e
```

###  Activate and Deactivate Environment

```shell
# activate myenv
conda activate myenv
# deactivate current env
conda deactivate
```

## Terminate python program

* exit()
  * Terminate python program
  * Should not be used in production code.
* quit()
  * same as exit()
* os.exit()
  * exit python program
* sys.exit()
  * Terminate python program
  * rises the SystemExit exception
  * can be used in production code

```python
import os
import sys
exit()
quit()
sys.exit("some thing went wronge")
os.exit(0)
```



### Python Tutorial

* Python is a multipurpose programming language.
* Machine learning and AI.
* Web development using a framework name django.(e.g youtube)
* Task Automation

### Tkinter GUI(Bro Code)

Tkinter is a module that is included with python.

```python
from tkinter import * # import all freatures
```

Widgets = GUI elements: button, textboxes, labels, images etc

Windows = serves as a container to hold or contain these widgets.

* Tkinter support photoimage

```python
icon = PhotoImage(file='logo.png')
```

#### Basic Window

```python
from tkinter import *
window = Tk()
winodw.geometry("420x420")
window.title("Bro Code first GUI program")

icon = PhotoImage(file='logo.png') # the icon to use
window.iconphoto(True,icon)
window.config(background='#5cfcff') # color hex code, or name

window.mainloop()
```

#### Label

label = an area widget that holds text and/or an image within a window

```python
form tkinter import *
window = Tk()
# also add a photo/image
photo = PhotoImage(file='person.png')
# this need to owner window and keyword argument
# compound option is needed for image and text
label = Label(window,
              text = 'Hello world',
              font=('Arial','bold'),
              fg='#00FF00',
              bg='black'
              relief=RAISED,
              bd=10,
              padx=20,
              image=photo,
              compund='bottom')
label.pack() # or label.place
window.mainloop()
```

### Button

button = you click it, then it does stuff

```python
def click():
    prrint("You clicked the button!")
window = Tk()
# command is the call back function
button = Button(window,
               text='click me',
               command=click,
               font=("Comic Sans",30),
               fg="#00FF00",
               bg="black",
               activeforeground="#00FF00",
               activebackground="black",
               state=DISABLE)
button.pack()
windows.mainloop()
```

### Entry Box

Entry Widget = textbox that accepts a single line of user input

```python
entry = Entry(window,
             font=("Arial",50))
entry.pack(side=LEFT) # bind to left side
```

get text

```python
entry.get() # will return the string inside the entry widget
```

Delete text.

```python
entry.delete(0,END) # will delete all charactor
```

Insert text

```python
entry.insert(0,"Text")
```

#### File Open Dialog

```python
from tkinter import *
from tkinter import filedialog

def openFile():
    # use askopenfile will also open the file, not recomented
    # with this method we can use a context manager
	filepath = filedialog.askopenfilename(initialdir="[AbsoulePath]"
                                     ,title="openFile"
                                      ,filetypes=
                                      [("text files","*.txt")
                                       ,("all files","*.*")])
    print(filepath)
window = Tk()
button = Button(text="Open"
               ,command=openFile
               )
button.pack()
window.mainloop()
```

Open the file(we can use a context manager):

```python
def openFile():
	filepath = filedialog.askopenfile()
    file = open(filepath,'r')
    print(file.read())
    file.close()
```

#### Text Area

```python
text = Text(window)
text.pack()
```

```python
text = Text(window)
# to get data
text.get("1.0",END)
```

```python
import tkinter as tk

root = tk.Tk()
T = tk.Text(root, height=2, width=30)
T.pack()
T.insert(tk.END, "Just a text Widget\nin two lines\n")
tk.mainloop()
```

#### Update Text Label

```python
my_label.config(text = my_text)
```

#### Command Line Interface (CLI)

```python
import sys

print('Number of arguments:', len(sys.argv), 'arguments.')
print('Argument List:', str(sys.argv))
```

```shell
# output
$ python test.py arg1 arg2 arg3
Number of arguments: 4 arguments.
Argument List: ['test.py', 'arg1', 'arg2', 'arg3']
```

### Python Class

```python
class Person:
  def __init__(self, name, age):
    self.name = name
    self.age = age

  def myfunc(self):
    print("Hello my name is " + self.name)

p1 = Person("John", 36)
p1.myfunc()
```

The `self` parameter is a reference to the current instance of the class, and is used to access variables that belong to the class.

Python in defaults the return to 'None'.

```python
a = f1()
if a is None:
	print("you did not return anything")
```

## Pip Environments

```
python3 -m venv tutorial-env
python -m venv .venv
```

```
/.venv/Scripts/activate.bat
/.venv/Scripts/deactivate.bat
```

```
pip freeze > requirements.txt
```

## Base64 Encoding

```python
import base64

message = "Python is fun"
message_bytes = message.encode('ascii')
base64_bytes = base64.b64encode(message_bytes)
base64_message = base64_bytes.decode('ascii')

print(base64_message)
```

### Base64 Decoding

```python
import base64

base64_message = 'UHl0aG9uIGlzIGZ1bg=='
base64_bytes = base64_message.encode('ascii')
message_bytes = base64.b64decode(base64_bytes)
message = message_bytes.decode('ascii')

print(message)
```

## Base16 Encoding(Hexadecimal)

```python
import base64

message_bytes = b'\x33\x32'
base16_bytes = base64.b16encode(message_bytes)
base64_message = base16_bytes.decode('ascii') # to convert it to string

print(base64_message)
```

## Time For Bench Marking

```python
import multiprocessing
import time

start = time.perf_counter()


def do_somthing():
    print("Sleeping 1 second..")
    time.sleep(1)
    print("done sleeping.")

if __name__=='__main__':
    do_somthing()
    do_somthing()
    finish = time.perf_counter()

    print(f'Finished in {round(finish-start,2)} second(s)')
```

