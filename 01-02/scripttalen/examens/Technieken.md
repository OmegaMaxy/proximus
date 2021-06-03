# FAQ

**Wat is dict()?**

```
Dict is een functie om keys en values uit een data type te halen om ze vervolgens aan elkaar toe te wijzen.
Voorbeeld:
a = (('John', 'Jenny'), a = ('John2', 'Jenny2'))
print(dict(a))
Resultaat: {'John': 'Jenny', 'John2': 'Jenny2'}
Andere syntax: result = {}
```

**Wat is zip()?**

```
Zip is een functie om waardes op dezelfde index van meerdere arrays in 1 tuple te steken.
Voorbeeld:
a = ("John", "Charles", "Mike")
b = ("Jenny", "Christy", "Monica")
x = zip(a, b)
print(tuple(x))
Resultaat: (('John', 'Jenny'), ('Charles', 'Christy'), ('Mike', 'Monica'))
```

**Wat is een tuple?**

```
Een tuple is een onveranderlijk data type dat lijkt op een array
Voorbeeld: ('John', 'Jenny')
```



# Technieken

## Simpel - drop last

`Source: 02-advanced-python\01-functions\01-drop-last (oefeningen)`

```python
def drop_last(xs, n=1):
    return xs[:-n]
```



## Simpel sorted()

`Source: 02-advanced-python\01-functions\02-longest-string (oefeningen)`

```python
def nth_longest_string(n, strings):
    return sorted(strings, key=len)[-n]
```



## Medium list comprehension - nested for loop

`Source: 02-list-comprehension\07-all-greater (oefeningen)`

```python
def all_greater(ns, ms):
    return all( [ n >= m for m in ms for n in ns ] )
```



## Medium list comprehension - priemgetallen/prime numbers

`Source: 02-list-comprehension\08-is-prime (oefeningen)`

```python
def is_prime(n):
    return n > 1 and all( [ n % k != 0 for k in range(2, n) ] )
```



## Simpel list comprehension

`Source: 02-list-comprehension\01-sum-squares (oefeningen)`

```python
def sum_squares(ns):
    return sum([ n ** 2 for n in ns ])
```



## Advanced arguments - Tail

`Source: 05-modules/shell-tools/01-echo (oefeningen)`

```python
#!/usr/bin/env python

import argparse
import sys


def create_parser():
    parser = argparse.ArgumentParser(prog='tail')
    parser.add_argument('file', help='file', nargs='?')
    parser.add_argument('-n', '--lines', help='number of lines to print', action='store', default=5, type=int)
    return parser


def print_tail(stream, n):
    lines = stream.readlines()
    for line in lines[-n:]:
        print(line, end="")

args = create_parser().parse_args()

if args.file:
    with open(args.file, 'r') as file:
        print_tail(file, args.lines)
else:
    print_tail(sys.stdin, args.lines)
```



## Simpel arguments - sys.argv

`Source: 05-modules/shell-tools/01-echo (oefeningen)`

```python
import sys


print(' '.join(sys.argv[1:]))
```



## JSON to XML

```python
import json

with open('input.json') as f:
    students = json.load(f)

with open('output.txt', 'w') as out:
    print('<students>', file=out)
    for entry in students:
        id, grades = entry['id'], entry['grades']
        formatted_grades = "".join( f"<grade>{x}</grade>" for x in grades )

        print(f'<student id="{id}"><grades>{formatted_grades}</grades></student>', file=out)
    print('</students>', file=out)
```



## Datums/Dates

`Source: exam/check-dates (iswleuven exam january)`

```python
# Write your solution in this file
from datetime import datetime

with open('input.txt') as input:
    data = input.readlines()
    with open('output.txt', 'w') as output:
        for line in data:
            try:
                date = datetime.strptime(line.strip(), '%d %B, %Y')
                output.write(f'{line.strip()}\n')
            except ValueError:
                print('test')
```



## Combinatie dict() en zip()

`Source: exam/countries (iswleuven exam january)`

```python
# Write your solution in this file

with open('countries.txt') as inputcountries:
    countrydata = inputcountries.read().splitlines()

with open('codes.txt') as inputcodes:
    codedata = inputcodes.read().splitlines()

combination = dict(zip(codedata, countrydata))

with open('output.txt', 'w') as output:
    for key in sorted(combination.keys()):
        output.write(f'{key} -> {combination[key]}\n')
```



## Meerdere csv files

`Source: exam/food-orders (iswleuven exam january)`

```python
# Write your solution in this file
import csv

with open('menu-options.csv') as csv_options:
    options = csv_options.read().strip().split(',')

    optionsdict = dict.fromkeys(options, 0)

with open('orders.csv') as csv_orders:
        orders = csv.reader(csv_orders, delimiter=',')
        for order in orders:
            for i in range(len(order)-1):
                if order[i+1] in optionsdict:
                    optionsdict[order[i+1]] += 1


sortoptions = sorted(optionsdict.items(), key=lambda x: x[1], reverse=True)

with open('output.txt', 'w') as output:
    for i in sortoptions:
        output.write(f'{i[0]} {i[1]}\n')
```



## re.sub() - Regex vervangen

`Source: exam/from-hex-to-dec (iswleuven exam january)`

````python
# Write your solution in this file
import re

with open('input.txt') as f:
    with open('output.txt', 'w') as out:
        for line in f:
            line = re.sub(r'0x([a-zA-Z]|[0-9])*', lambda m:str(int(m.group(), 16)), line)
            out.write(line)
````



## Sorteren en JSON

`Source: exam/total-kms-run (iswleuven exam january)`

```python
# Write your solution in this file
import json

with open('input.json') as f:
    runners = json.load(f)

result = {}

for runner in runners:
    name = runner['name']
    kms = runner['kms']
    if name in result:
        result[name] += sum(kms)
    else:
        result[name] = sum(kms)

sortrunners = sorted(result.items(), key=lambda x: x[1], reverse=True)

with open('output.txt', 'w') as output:
    for i in sortrunners:
        output.write(f'{i[0]} {i[1]}\n')
```



## Gemiddelde

`Source: old-exams/blanket(oefeningen)`

```python
from datetime import datetime
import json
from statistics import mean


with open('input.txt') as file:
    data = [ (datetime.strptime(date, '%d/%m/%Y'), temps) for date, temps in json.load(file).items() ]

for _, temps in sorted(data, key=lambda p: p[0]):
    print(round(mean(temps)))
   
```



# Snelheid

```python
import hashlib
from os import write

pas = {}
with open('dictionary.txt') as d:
    for line in d.readlines():
        line = line.strip()
        line2 = hashlib.sha256(line.encode('utf-8')).hexdigest()
        pas[line2] = line


def check_hash(str):
    return pas.get(str)


with open('hashed.txt') as f:
    with open('output.txt', 'w') as out:
        for line in f.readlines():
            line = line.strip()
            id, hash = line.split(' ')
            print(f'{id} {check_hash(hash)}', file=out)
```



# Online Tools

* https://regexr.com/
* https://realpython.com/
* https://www.w3schools.com/

``Made by dazzy 2021``

