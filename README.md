
![header](https://user-images.githubusercontent.com/30802364/69887234-e65abc80-12dd-11ea-8111-3643447fd4ca.PNG)

# ID Card Generator Project P001 CODE ESI

### Imports


```python
import pandas as pd 
import pdfkit
from datetime import datetime, timedelta
import code128
import random
```

### Functions


```python
#generate costum html file using the arguments
def HTMLgen(code,first_name,last_name,function,date_of_birth,picture,expiration_date):
    html= r"""<!doctype html><meta charset="utf-8"><link rel="stylesheet" href="../RES/card.css"><body><div class="face face-front" ><img src="../RES/front.png"></div><div id="infoi"><img src="@picture" height="89.5" width="83" />
        <div style="margin-left: 1.3cm;margin-top: -0.6cm;">
            <br>
            <div style="font-size: 0.7em;margin-top: 5%;font-family: sans-serif;color: aliceblue;text-transform: uppercase;"><b>@fname</b> @lname</div><br>
        <div style="font-size: 0.7em;margin-top: -0.4cm;font-family: sans-serif;color: aliceblue;text-t ransform: capitalize;">@function</div>
        </div>
    </div>
    <div id="info">
        <br><div style="font-size: 0.7em;margin-top: 0.6%;font-family: sans-serif;text-transform: uppercase;">@code</div>
        <br><div style="font-size: 0.7em;margin-top: -0.6%;font-family: sans-serif;text-transform: capitalize;">@date_of_birth</div>
        <br><div style="font-size: 0.7em;margin-top: -0.6%;font-family: sans-serif;text-transform: capitalize;">@expiration_date</div>
    </div>
    <div id="BARCODE"><img src="../RES/bar.PNG"  height="20" width="120"/></div>

</body>"""
    html = html.replace("@picture",picture)
    html = html.replace("@code",str(code))
    html = html.replace("@fname",first_name)
    html = html.replace("@lname",last_name)
    html = html.replace("@function",function)
    html = html.replace("@date_of_birth",date_of_birth)
    html = html.replace("@expiration_date",expiration_date)
    f= open("index.html","w")
    f.write(html)
    f.close()
    return 
#generate a file bar.png contains barcode of the 10 digits code  
def BARgen(code):
    code128.image(code).save("../RES/bar.png")
    return
#generate costum pdf file using the existing html file // code argument is only to name the file generated
def PDFgen(code):
    config = pdfkit.configuration(wkhtmltopdf='C:\\Program Files\\wkhtmltopdf\\bin\\wkhtmltopdf.exe')
    options = {'dpi': 365,'margin-top': '0in','margin-bottom': '0in','margin-right': '0in','margin-left': '0in','page-size': 'A8',"orientation": "Landscape",'disable-smart-shrinking': ''}
    pdfkit.from_file('index.html', str(code)+".pdf",configuration=config , options = options)
    return       
#complete with random digits an integer x until it contains 10 digits
def complete(x):
    x=str(x)
    while len(x)<10:
            x+=str(random.randint(0,9))
    return int(x)
```

### Import Dataset


```python
data=pd.read_excel("../RES/exo_dataset_P001.xlsx")
```

### Show Sample


```python
data.head(5)
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>code</th>
      <th>first_name</th>
      <th>last_name</th>
      <th>function</th>
      <th>date_of_birth</th>
      <th>submission_date</th>
      <th>picture</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5591927432</td>
      <td>Prent</td>
      <td>Hickenbottom</td>
      <td>Senior Editor</td>
      <td>4/3/1993</td>
      <td>22/1/2019</td>
      <td>https://robohash.org/quidemsaepequi.png?size=3...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5071682362</td>
      <td>Jard</td>
      <td>Stein</td>
      <td>Geological Engineer</td>
      <td>15/7/1992</td>
      <td>10/4/2019</td>
      <td>https://robohash.org/teneturautsunt.png?size=3...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6295398980</td>
      <td>Levon</td>
      <td>Noen</td>
      <td>Research Assistant IV</td>
      <td>12/10/1999</td>
      <td>27/3/2019</td>
      <td>https://robohash.org/quaeetsed.png?size=300x30...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3718164078</td>
      <td>Nonah</td>
      <td>Scoggin</td>
      <td>Junior Executive</td>
      <td>9/7/2000</td>
      <td>21/1/2019</td>
      <td>https://robohash.org/debitisinciduntdolore.png...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2261412840</td>
      <td>Jameson</td>
      <td>Hugueville</td>
      <td>Teacher</td>
      <td>5/12/1991</td>
      <td>1/2/2019</td>
      <td>https://robohash.org/facilisrerumad.png?size=3...</td>
    </tr>
  </tbody>
</table>
</div>

some of the code column entries are only 9 digits or less, so we will add some random degits to them.


```python
data['code'] = data['code'].apply(lambda x: complete(x))
```


```python
data.dtypes
```




   code                int64<br>
   first_name         object<br>
   last_name          object<br>
   function           object<br>
   date_of_birth      object<br>
   submission_date    object<br>
   picture            object<br>
   dtype: object<br>



### convert to datetime elements 


```python
data['submission_date']=pd.to_datetime(data['submission_date'])
data['date_of_birth'] = pd.to_datetime(data['date_of_birth'])
```


```python
data.dtypes
```




   code                        int64<br>
   first_name                 object<br>
   last_name                  object<br>
   function                   object<br>
   date_of_birth      datetime64[ns]<br>
   submission_date    datetime64[ns]<br>
   picture                    object<br>
   dtype: object<br>




```python
data.head(5)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>code</th>
      <th>first_name</th>
      <th>last_name</th>
      <th>function</th>
      <th>date_of_birth</th>
      <th>submission_date</th>
      <th>picture</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5591927432</td>
      <td>Prent</td>
      <td>Hickenbottom</td>
      <td>Senior Editor</td>
      <td>1993-04-03</td>
      <td>2019-01-22</td>
      <td>https://robohash.org/quidemsaepequi.png?size=3...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5071682362</td>
      <td>Jard</td>
      <td>Stein</td>
      <td>Geological Engineer</td>
      <td>1992-07-15</td>
      <td>2019-10-04</td>
      <td>https://robohash.org/teneturautsunt.png?size=3...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6295398980</td>
      <td>Levon</td>
      <td>Noen</td>
      <td>Research Assistant IV</td>
      <td>1999-12-10</td>
      <td>2019-03-27</td>
      <td>https://robohash.org/quaeetsed.png?size=300x30...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3718164078</td>
      <td>Nonah</td>
      <td>Scoggin</td>
      <td>Junior Executive</td>
      <td>2000-09-07</td>
      <td>2019-01-21</td>
      <td>https://robohash.org/debitisinciduntdolore.png...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2261412840</td>
      <td>Jameson</td>
      <td>Hugueville</td>
      <td>Teacher</td>
      <td>1991-05-12</td>
      <td>2019-01-02</td>
      <td>https://robohash.org/facilisrerumad.png?size=3...</td>
    </tr>
  </tbody>
</table>
</div>



### add expiration date column (submission date + 15 days) 


```python
data["expiration_date"] = data["submission_date"] + timedelta(days=15)
```

### Controlling date form DD-MM-YYYY


```python
data['date_of_birth']=data['date_of_birth'].dt.strftime('%d-%m-%Y')
data['submission_date']=data['submission_date'].dt.strftime('%d-%m-%Y')
data['expiration_date']=data['expiration_date'].dt.strftime('%d-%m-%Y')
```


```python
data.head(5)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>code</th>
      <th>first_name</th>
      <th>last_name</th>
      <th>function</th>
      <th>date_of_birth</th>
      <th>submission_date</th>
      <th>picture</th>
      <th>expiration_date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5591927432</td>
      <td>Prent</td>
      <td>Hickenbottom</td>
      <td>Senior Editor</td>
      <td>03-04-1993</td>
      <td>22-01-2019</td>
      <td>https://robohash.org/quidemsaepequi.png?size=3...</td>
      <td>06-02-2019</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5071682362</td>
      <td>Jard</td>
      <td>Stein</td>
      <td>Geological Engineer</td>
      <td>15-07-1992</td>
      <td>04-10-2019</td>
      <td>https://robohash.org/teneturautsunt.png?size=3...</td>
      <td>19-10-2019</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6295398980</td>
      <td>Levon</td>
      <td>Noen</td>
      <td>Research Assistant IV</td>
      <td>10-12-1999</td>
      <td>27-03-2019</td>
      <td>https://robohash.org/quaeetsed.png?size=300x30...</td>
      <td>11-04-2019</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3718164078</td>
      <td>Nonah</td>
      <td>Scoggin</td>
      <td>Junior Executive</td>
      <td>07-09-2000</td>
      <td>21-01-2019</td>
      <td>https://robohash.org/debitisinciduntdolore.png...</td>
      <td>05-02-2019</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2261412840</td>
      <td>Jameson</td>
      <td>Hugueville</td>
      <td>Teacher</td>
      <td>12-05-1991</td>
      <td>02-01-2019</td>
      <td>https://robohash.org/facilisrerumad.png?size=3...</td>
      <td>17-01-2019</td>
    </tr>
  </tbody>
</table>
</div>



### Generating Costumized Pdf files from the dataset


```python
for index, row in data.head(n=5).iterrows()    :
    code=row[0]
    fname = row[1]
    lname = row[2]
    func  = row[3]
    dob   = row[4]
    ed    = row[5]
    pic   = row[6]
    BARgen(code)
    HTMLgen(code,fname,lname,func,dob,pic,ed)
    PDFgen(code)
    
```

   Loading pages (1/6)<br>
    Counting pages (2/6)<br>
     Resolving links (4/6)<br>
      Loading headers and footers (5/6)<br>
       Printing pages (6/6)<br>
        Done <br>
   
   #### **generated 5 pdf files with thier corresponding enteries**

<p float="left">
<img src="https://user-images.githubusercontent.com/30802364/69887237-e9ee4380-12dd-11ea-9df4-17475b27a3c1.PNG" width="250"/><img align="right" src="https://user-images.githubusercontent.com/30802364/69887338-5cf7ba00-12de-11ea-933c-f4d975633990.gif" width="450" />
</p>

### Thank you
