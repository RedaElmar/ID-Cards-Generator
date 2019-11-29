
<img src="../Res/header.png" />

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
data.head(40)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
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
    <tr>
      <th>5</th>
      <td>7226393395</td>
      <td>Lynnett</td>
      <td>Hambrick</td>
      <td>Software Engineer II</td>
      <td>17/5/1998</td>
      <td>23/2/2019</td>
      <td>https://robohash.org/eosculpaquam.png?size=300...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1088777325</td>
      <td>Davie</td>
      <td>Wetwood</td>
      <td>Recruiter</td>
      <td>1/3/1993</td>
      <td>14/4/2019</td>
      <td>https://robohash.org/cumofficiarepudiandae.png...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>4971860363</td>
      <td>Tabor</td>
      <td>Harrower</td>
      <td>Senior Financial Analyst</td>
      <td>9/1/1995</td>
      <td>27/3/2019</td>
      <td>https://robohash.org/etdoloribusreiciendis.png...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9695067972</td>
      <td>Abagail</td>
      <td>Jouannin</td>
      <td>Programmer IV</td>
      <td>5/4/1994</td>
      <td>11/4/2019</td>
      <td>https://robohash.org/suntliberoveritatis.png?s...</td>
    </tr>
    <tr>
      <th>9</th>
      <td>4931427782</td>
      <td>Shantee</td>
      <td>Cordero</td>
      <td>Electrical Engineer</td>
      <td>20/6/2000</td>
      <td>14/4/2019</td>
      <td>https://robohash.org/suscipithicest.png?size=3...</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2561912337</td>
      <td>Danny</td>
      <td>Garken</td>
      <td>Analog Circuit Design manager</td>
      <td>17/4/1991</td>
      <td>17/2/2019</td>
      <td>https://robohash.org/etetconsequatur.png?size=...</td>
    </tr>
    <tr>
      <th>11</th>
      <td>4256157018</td>
      <td>Rosaleen</td>
      <td>Domonkos</td>
      <td>Web Designer II</td>
      <td>16/10/1996</td>
      <td>3/2/2019</td>
      <td>https://robohash.org/reprehenderitdebitisnobis...</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2055115286</td>
      <td>Kristine</td>
      <td>Devaney</td>
      <td>Junior Executive</td>
      <td>4/10/1992</td>
      <td>7/1/2019</td>
      <td>https://robohash.org/seddictaab.png?size=300x3...</td>
    </tr>
    <tr>
      <th>13</th>
      <td>7984595380</td>
      <td>Carrie</td>
      <td>Dumper</td>
      <td>Chief Design Engineer</td>
      <td>22/10/1994</td>
      <td>22/3/2019</td>
      <td>https://robohash.org/undeidveritatis.png?size=...</td>
    </tr>
    <tr>
      <th>14</th>
      <td>3386631134</td>
      <td>Luke</td>
      <td>Bickerstaff</td>
      <td>Media Manager III</td>
      <td>25/9/1993</td>
      <td>27/3/2019</td>
      <td>https://robohash.org/doloresillumvitae.png?siz...</td>
    </tr>
    <tr>
      <th>15</th>
      <td>5253322272</td>
      <td>Tabatha</td>
      <td>Catley</td>
      <td>Software Test Engineer III</td>
      <td>26/11/1995</td>
      <td>12/2/2019</td>
      <td>https://robohash.org/autiustoreiciendis.png?si...</td>
    </tr>
    <tr>
      <th>16</th>
      <td>4738968260</td>
      <td>Oberon</td>
      <td>Beazer</td>
      <td>Community Outreach Specialist</td>
      <td>14/1/1996</td>
      <td>16/2/2019</td>
      <td>https://robohash.org/utinmolestias.png?size=30...</td>
    </tr>
    <tr>
      <th>17</th>
      <td>1425469388</td>
      <td>Kristos</td>
      <td>Lux</td>
      <td>Geological Engineer</td>
      <td>31/7/1999</td>
      <td>7/2/2019</td>
      <td>https://robohash.org/quodvoluptatesdebitis.png...</td>
    </tr>
    <tr>
      <th>18</th>
      <td>9326331857</td>
      <td>Lucina</td>
      <td>Lanahan</td>
      <td>Actuary</td>
      <td>25/2/1994</td>
      <td>7/4/2019</td>
      <td>https://robohash.org/porroetnobis.png?size=300...</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2860174680</td>
      <td>Maure</td>
      <td>Davies</td>
      <td>VP Marketing</td>
      <td>4/12/1992</td>
      <td>24/2/2019</td>
      <td>https://robohash.org/consequatursitcupiditate....</td>
    </tr>
    <tr>
      <th>20</th>
      <td>7461604813</td>
      <td>Harold</td>
      <td>Kordas</td>
      <td>Programmer Analyst III</td>
      <td>22/7/1995</td>
      <td>24/3/2019</td>
      <td>https://robohash.org/inciduntomnisqui.png?size...</td>
    </tr>
    <tr>
      <th>21</th>
      <td>7341605200</td>
      <td>Dmitri</td>
      <td>Lethem</td>
      <td>Marketing Assistant</td>
      <td>20/12/1996</td>
      <td>28/1/2019</td>
      <td>https://robohash.org/quoadaut.png?size=300x300...</td>
    </tr>
    <tr>
      <th>22</th>
      <td>4777281970</td>
      <td>Alejandra</td>
      <td>MacCauley</td>
      <td>Accounting Assistant IV</td>
      <td>9/4/1996</td>
      <td>4/1/2019</td>
      <td>https://robohash.org/nemosimiliquevelit.png?si...</td>
    </tr>
    <tr>
      <th>23</th>
      <td>4157762282</td>
      <td>Carlynne</td>
      <td>Rickett</td>
      <td>Senior Developer</td>
      <td>12/6/1997</td>
      <td>15/3/2019</td>
      <td>https://robohash.org/voluptatemvelexcepturi.pn...</td>
    </tr>
    <tr>
      <th>24</th>
      <td>1081781408</td>
      <td>Carole</td>
      <td>Bende</td>
      <td>Senior Editor</td>
      <td>3/10/1996</td>
      <td>24/2/2019</td>
      <td>https://robohash.org/doloriureeum.png?size=300...</td>
    </tr>
    <tr>
      <th>25</th>
      <td>7557982436</td>
      <td>Jeanine</td>
      <td>Fortie</td>
      <td>Nurse Practicioner</td>
      <td>28/8/1995</td>
      <td>30/1/2019</td>
      <td>https://robohash.org/inciduntcumexercitationem...</td>
    </tr>
    <tr>
      <th>26</th>
      <td>8352361872</td>
      <td>Vanya</td>
      <td>Benneton</td>
      <td>Structural Engineer</td>
      <td>16/6/1996</td>
      <td>17/2/2019</td>
      <td>https://robohash.org/nesciuntsitdebitis.png?si...</td>
    </tr>
    <tr>
      <th>27</th>
      <td>8982911871</td>
      <td>Ingeborg</td>
      <td>Spada</td>
      <td>Social Worker</td>
      <td>27/7/1992</td>
      <td>1/1/2019</td>
      <td>https://robohash.org/voluptatemdolorofficiis.p...</td>
    </tr>
    <tr>
      <th>28</th>
      <td>1890373168</td>
      <td>Jewelle</td>
      <td>Grelik</td>
      <td>Senior Sales Associate</td>
      <td>2/8/1993</td>
      <td>11/4/2019</td>
      <td>https://robohash.org/quiaestnon.png?size=300x3...</td>
    </tr>
    <tr>
      <th>29</th>
      <td>597199779</td>
      <td>Ellissa</td>
      <td>Dmtrovic</td>
      <td>Senior Quality Engineer</td>
      <td>15/4/1998</td>
      <td>27/3/2019</td>
      <td>https://robohash.org/essererumvoluptatem.png?s...</td>
    </tr>
    <tr>
      <th>30</th>
      <td>8643041742</td>
      <td>Nadya</td>
      <td>Gamon</td>
      <td>Senior Financial Analyst</td>
      <td>15/12/1992</td>
      <td>17/3/2019</td>
      <td>https://robohash.org/etrerumrepudiandae.png?si...</td>
    </tr>
    <tr>
      <th>31</th>
      <td>7399610088</td>
      <td>Jorry</td>
      <td>Baudasso</td>
      <td>VP Sales</td>
      <td>7/12/1992</td>
      <td>2/3/2019</td>
      <td>https://robohash.org/rerumsedvel.png?size=300x...</td>
    </tr>
    <tr>
      <th>32</th>
      <td>5255564982</td>
      <td>Tamar</td>
      <td>Nutting</td>
      <td>Information Systems Manager</td>
      <td>14/8/1993</td>
      <td>23/3/2019</td>
      <td>https://robohash.org/oditenimlaborum.png?size=...</td>
    </tr>
    <tr>
      <th>33</th>
      <td>8557336683</td>
      <td>Shaun</td>
      <td>Kemet</td>
      <td>GIS Technical Architect</td>
      <td>7/1/1992</td>
      <td>29/3/2019</td>
      <td>https://robohash.org/sintpariaturut.png?size=3...</td>
    </tr>
    <tr>
      <th>34</th>
      <td>6784154681</td>
      <td>Auguste</td>
      <td>Weetch</td>
      <td>Structural Engineer</td>
      <td>26/11/1999</td>
      <td>26/1/2019</td>
      <td>https://robohash.org/utasperiorescum.png?size=...</td>
    </tr>
    <tr>
      <th>35</th>
      <td>8124097216</td>
      <td>Theressa</td>
      <td>Yakubovich</td>
      <td>Accountant IV</td>
      <td>6/5/1993</td>
      <td>22/3/2019</td>
      <td>https://robohash.org/quibusdamnontotam.png?siz...</td>
    </tr>
    <tr>
      <th>36</th>
      <td>6206125874</td>
      <td>Zora</td>
      <td>Kingdom</td>
      <td>Clinical Specialist</td>
      <td>26/2/1993</td>
      <td>13/4/2019</td>
      <td>https://robohash.org/praesentiumquiafacere.png...</td>
    </tr>
    <tr>
      <th>37</th>
      <td>4808909243</td>
      <td>Elsworth</td>
      <td>Wisham</td>
      <td>Data Coordiator</td>
      <td>10/4/1998</td>
      <td>18/2/2019</td>
      <td>https://robohash.org/nostrumexcepturirem.png?s...</td>
    </tr>
    <tr>
      <th>38</th>
      <td>4958537813</td>
      <td>Jaimie</td>
      <td>Caisley</td>
      <td>Junior Executive</td>
      <td>15/9/1995</td>
      <td>13/4/2019</td>
      <td>https://robohash.org/suntsedsimilique.png?size...</td>
    </tr>
    <tr>
      <th>39</th>
      <td>7453768691</td>
      <td>Brittni</td>
      <td>Topper</td>
      <td>Electrical Engineer</td>
      <td>7/4/1995</td>
      <td>4/3/2019</td>
      <td>https://robohash.org/rematquevoluptatum.png?si...</td>
    </tr>
  </tbody>
</table>
</div>



As you can see some of the code entries are only 9 digits or less, so we will add some random degits to them.


```python
data['code'] = data['code'].apply(lambda x: complete(x))
```


```python
data.dtypes
```




    code                int64
    first_name         object
    last_name          object
    function           object
    date_of_birth      object
    submission_date    object
    picture            object
    dtype: object



### convert to datetime elements 


```python
data['submission_date']=pd.to_datetime(data['submission_date'])
data['date_of_birth'] = pd.to_datetime(data['date_of_birth'])
```


```python
data.dtypes
```




    code                        int64
    first_name                 object
    last_name                  object
    function                   object
    date_of_birth      datetime64[ns]
    submission_date    datetime64[ns]
    picture                    object
    dtype: object




```python
data.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
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
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
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

    Loading pages (1/6)
    Counting pages (2/6)                                               
    Resolving links (4/6)                                                       
    Loading headers and footers (5/6)                                           
    Printing pages (6/6)
    Done                                                                      
    Loading pages (1/6)
    Counting pages (2/6)                                               
    Resolving links (4/6)                                                       
    Loading headers and footers (5/6)                                           
    Printing pages (6/6)
    Done                                                                      
    Loading pages (1/6)
    Counting pages (2/6)                                               
    Resolving links (4/6)                                                       
    Loading headers and footers (5/6)                                           
    Printing pages (6/6)
    Done                                                                      
    Loading pages (1/6)
    Counting pages (2/6)                                               
    Resolving links (4/6)                                                       
    Loading headers and footers (5/6)                                           
    Printing pages (6/6)
    Done                                                                      
    Loading pages (1/6)
    Counting pages (2/6)                                               
    Resolving links (4/6)                                                       
    Loading headers and footers (5/6)                                           
    Printing pages (6/6)
    Done                                                                      
    

#### **generated 5 pdf files with thier corresponding enteries**

<p float="left">
<img align="left" src="../Res/PDFS.png" width="250"/><img src="../Res/GIF.gif" width="450" />
</p>

### done
