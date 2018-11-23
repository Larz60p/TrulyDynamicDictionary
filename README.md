# TrulyDynamicDictionary

## Create nested dictionaries on the fly

This class allows creation of Dynamic Nested dictionaries, and includes a formatted print routine for same.

The built in example 'testit' builds the following dictionary dynamically:

```python
CityList = {
    'Boston': {
        'Resturants': {
            'Spoke Wine Bar': {
                'Addr1': '89 Holland St',
                'City': 'Sommerville',
                'ZipCode': '02144',
                'Phone': '617-718-9463'
            },
            'Highland Kitchen': {
                'Addr1': '150 Highland Ave',
                'City': 'Sommerville',
                'ZipCode': '02144',
                'Phone': '617-625-1131'
            }
        }
    }
}
```

The code is simple:
```python
Copyright 2018 Larz60+

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE

import os


class CreateDict:
    def __init__(self):
        os.chdir(os.path.abspath(os.path.dirname(__file__)))

    def new_dict(self, dictname):
        setattr(self, dictname, {})

    def add_node(self, parent, nodename):
        node = parent[nodename] = {}
        return node
    
    def add_cell(self, nodename, cellname, value):
        cell =  nodename[cellname] = value
        return cell

    def display_dict(self, dictname, level=0):
        indent = " " * (4 * level)
        for key, value in dictname.items():
            if isinstance(value, dict):
                print(f'\n{indent}{key}')
                level += 1
                self.display_dict(value, level)
            else:
                print(f'{indent}{key}: {value}')
            if level > 0:
                level -= 1


def testit():
    cd = CreateDict()
    cd.new_dict('CityList')
    boston = cd.add_node(cd.CityList, 'Boston')
    bos_resturants = cd.add_node(boston, 'Resturants')
    spoke = cd.add_node(bos_resturants, 'Spoke Wine Bar')
    cd.add_cell(spoke, 'Addr1', '89 Holland St')
    cd.add_cell(spoke, 'City', 'Sommerville')
    cd.add_cell(spoke, 'Addr1', '02144')
    cd.add_cell(spoke, 'Phone', '617-718-9463')

    highland = cd.add_node(bos_resturants, 'Highland Kitchen')
    cd.add_cell(highland, 'Addr1', '150 Highland Ave')
    cd.add_cell(highland, 'City', 'Sommerville')
    cd.add_cell(highland, 'ZipCode', '02144')
    cd.add_cell(highland, 'Phone', '617-625-1131')

    print(f'\nCityList Dictionary')
    cd.display_dict(cd.CityList)
    print(f'\nraw data: {cd.CityList}')

if __name__ == '__main__':
    testit()
```

## Test results
```python
Boston

    Resturants

        Spoke Wine Bar
            Addr1: 02144
            City: Sommerville
            Phone: 617-718-9463

        Highland Kitchen
            Addr1: 150 Highland Ave
            City: Sommerville
            ZipCode: 02144
            Phone: 617-625-1131

raw data: {'Boston': {'Resturants': {'Spoke Wine Bar': {'Addr1': '02144', 'City': 'Sommerville', 'Phone': '617-718-9463'}, 'Highland Kitchen': {'Addr1': '150 Highland Ave', 'City': 'Sommerville', 'ZipCode': '02144', 'Phone': '617-625-1131'}}}}
```
