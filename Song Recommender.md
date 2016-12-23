
## Builing a Song Recommendation Model
# Shivam Panchal
**(shivam.panchal@mail.com)**

**importing the necessary packages and setting the working directory**


```python
import graphlab
```

**Setting the working directory**


```python
import os
print os.getcwd()
os.chdir('C://users/Shivam Panchal/Desktop/Song Recommeder')
```

    C:\Users\Shivam Panchal
    


```python
songs_data = graphlab.SFrame("song_data.gl")
```

    This non-commercial license of GraphLab Create for academic use is assigned to shivamvaish09@gmail.com and will expire on October 16, 2017.
    

    [INFO] graphlab.cython.cy_server: GraphLab Create v2.1 started. Logging: C:\Users\SHIVAM~1\AppData\Local\Temp\graphlab_server_1482310838.log.0
    


```python
songs_data.head()
```




<div style="max-height:1000px;max-width:1500px;overflow:auto;"><table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">user_id</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">song_id</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">listen_count</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">title</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">artist</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">b80344d063b5ccb3212f76538<br>f3d9e43d87dca9e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOAKIMP12A8C130995</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">The Cove</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Jack Johnson</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">b80344d063b5ccb3212f76538<br>f3d9e43d87dca9e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOBBMDR12A8C13253B</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Entre Dos Aguas</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Paco De Lucia</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">b80344d063b5ccb3212f76538<br>f3d9e43d87dca9e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOBXHDL12A81C204C0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Stronger</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Kanye West</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">b80344d063b5ccb3212f76538<br>f3d9e43d87dca9e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOBYHAJ12A6701BF1D</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Constellations</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Jack Johnson</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">b80344d063b5ccb3212f76538<br>f3d9e43d87dca9e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SODACBL12A8C13C273</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Learn To Fly</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Foo Fighters</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">b80344d063b5ccb3212f76538<br>f3d9e43d87dca9e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SODDNQT12A6D4F5F7E</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">5</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Apuesta Por El Rock 'N'<br>Roll ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Héroes del Silencio</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">b80344d063b5ccb3212f76538<br>f3d9e43d87dca9e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SODXRTY12AB0180F3B</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Paper Gangsta</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Lady GaGa</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">b80344d063b5ccb3212f76538<br>f3d9e43d87dca9e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOFGUAY12AB017B0A8</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Stacked Actors</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Foo Fighters</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">b80344d063b5ccb3212f76538<br>f3d9e43d87dca9e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOFRQTD12A81C233C0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Sehr kosmisch</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Harmonia</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">b80344d063b5ccb3212f76538<br>f3d9e43d87dca9e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOHQWYZ12A6D4FA701</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Heaven's gonna burn your<br>eyes ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Thievery Corporation<br>feat. Emiliana Torrini ...</td>
    </tr>
</table>
<table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">song</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">The Cove - Jack Johnson</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Entre Dos Aguas - Paco De<br>Lucia ...</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Stronger - Kanye West</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Constellations - Jack<br>Johnson ...</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Learn To Fly - Foo<br>Fighters ...</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Apuesta Por El Rock 'N'<br>Roll - Héroes del ...</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Paper Gangsta - Lady GaGa</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Stacked Actors - Foo<br>Fighters ...</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Sehr kosmisch - Harmonia</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Heaven's gonna burn your<br>eyes - Thievery ...</td>
    </tr>
</table>
[10 rows x 6 columns]<br/>
</div>




```python
graphlab.canvas.set_target("ipynb")
```


```python
songs_data["song"].show()
```




```python
len(songs_data)
```




    1116609




```python
# Count the number of users
users = songs_data["user_id"].unique()
```


```python
users.head()
```




    dtype: str
    Rows: 10
    ['c66c10a9567f0d82ff31441a9fd5063e5cd9dfe8', '279292bb36dbfc7f505e36ebf038c81eb1d1d63e', 'c067c22072a17d33310d7223d7b79f819e48cf42', 'f6c596a519698c97f1591ad89f540d76f6a04f1a', '696787172dd3f5169dc94deef97e427cee86147d', '3a7111f4cdf3c5a85fd4053e3cc2333562e1e0cb', '31f6fd9d9936adb9f7d0f157fd960c0a676ccfd6', '532e98155cbfd1e1a474a28ed96e59e50f7c5baf', 'ee43b175ed753b2e2bce806c903d4661ad351a91', 'e372c27f6cb071518ae500589ae02c126954c148']




```python
len(users)
```




    66346



**Splitting the data into training and test datasets**


```python
train_data,test_data =songs_data.random_split(.8, seed=0)
```

## Popularity Based Recommender


```python
    popularity_model = graphlab.popularity_recommender.create(train_data,user_id = "user_id", item_id = "song")
```


<pre>Recsys training: model = popularity</pre>



<pre>Warning: Ignoring columns song_id, listen_count, title, artist;</pre>



<pre>    To use one of these as a target column, set target = <column_name></pre>



<pre>    and use a method that allows the use of a target.</pre>



<pre>Preparing data set.</pre>



<pre>    Data has 893580 observations with 66085 users and 9952 items.</pre>



<pre>    Data prepared in: 1.93675s</pre>



<pre>893580 observations to process; with 9952 unique items.</pre>



```python
#use the popularity model to make some predictions
popularity_model.recommend(users=[users[0]])
```




<div style="max-height:1000px;max-width:1500px;overflow:auto;"><table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">user_id</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">song</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">score</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">rank</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">c66c10a9567f0d82ff31441a9<br>fd5063e5cd9dfe8 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Sehr kosmisch - Harmonia</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">4754.0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">c66c10a9567f0d82ff31441a9<br>fd5063e5cd9dfe8 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Undo - Björk</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">4227.0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">c66c10a9567f0d82ff31441a9<br>fd5063e5cd9dfe8 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">You're The One - Dwight<br>Yoakam ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3781.0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">c66c10a9567f0d82ff31441a9<br>fd5063e5cd9dfe8 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Dog Days Are Over (Radio<br>Edit) - Florence + The ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3633.0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">4</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">c66c10a9567f0d82ff31441a9<br>fd5063e5cd9dfe8 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Revelry - Kings Of Leon</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3527.0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">5</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">c66c10a9567f0d82ff31441a9<br>fd5063e5cd9dfe8 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Horn Concerto No. 4 in E<br>flat K495: II. Romance ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3161.0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">6</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">c66c10a9567f0d82ff31441a9<br>fd5063e5cd9dfe8 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Secrets - OneRepublic</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3148.0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">7</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">c66c10a9567f0d82ff31441a9<br>fd5063e5cd9dfe8 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Fireflies - Charttraxx<br>Karaoke ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2532.0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">8</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">c66c10a9567f0d82ff31441a9<br>fd5063e5cd9dfe8 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Tive Sim - Cartola</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2521.0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">9</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">c66c10a9567f0d82ff31441a9<br>fd5063e5cd9dfe8 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Drop The World - Lil<br>Wayne / Eminem ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2053.0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">10</td>
    </tr>
</table>
[10 rows x 4 columns]<br/>
</div>




```python
#use the popularity model to make some predictions
popularity_model.recommend(users=[users[1]])
```




<div style="max-height:1000px;max-width:1500px;overflow:auto;"><table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">user_id</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">song</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">score</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">rank</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">279292bb36dbfc7f505e36ebf<br>038c81eb1d1d63e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Sehr kosmisch - Harmonia</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">4754.0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">279292bb36dbfc7f505e36ebf<br>038c81eb1d1d63e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Undo - Björk</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">4227.0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">279292bb36dbfc7f505e36ebf<br>038c81eb1d1d63e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">You're The One - Dwight<br>Yoakam ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3781.0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">279292bb36dbfc7f505e36ebf<br>038c81eb1d1d63e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Dog Days Are Over (Radio<br>Edit) - Florence + The ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3633.0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">4</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">279292bb36dbfc7f505e36ebf<br>038c81eb1d1d63e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Revelry - Kings Of Leon</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3527.0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">5</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">279292bb36dbfc7f505e36ebf<br>038c81eb1d1d63e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Horn Concerto No. 4 in E<br>flat K495: II. Romance ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3161.0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">6</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">279292bb36dbfc7f505e36ebf<br>038c81eb1d1d63e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Secrets - OneRepublic</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3148.0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">7</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">279292bb36dbfc7f505e36ebf<br>038c81eb1d1d63e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Hey_ Soul Sister - Train</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2538.0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">8</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">279292bb36dbfc7f505e36ebf<br>038c81eb1d1d63e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Fireflies - Charttraxx<br>Karaoke ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2532.0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">9</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">279292bb36dbfc7f505e36ebf<br>038c81eb1d1d63e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Tive Sim - Cartola</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2521.0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">10</td>
    </tr>
</table>
[10 rows x 4 columns]<br/>
</div>



** Conclusion: everyone gets recommended with the same songs since this recommender is built on popularity **


# Personalized Recommender


```python
personalized_model = graphlab.item_similarity_recommender.create(train_data,user_id="user_id",item_id="song")
```


<pre>Recsys training: model = item_similarity</pre>



<pre>Warning: Ignoring columns song_id, listen_count, title, artist;</pre>



<pre>    To use one of these as a target column, set target = <column_name></pre>



<pre>    and use a method that allows the use of a target.</pre>



<pre>Preparing data set.</pre>



<pre>    Data has 893580 observations with 66085 users and 9952 items.</pre>



<pre>    Data prepared in: 1.8433s</pre>



<pre>Training model from provided data.</pre>



<pre>Gathering per-item and per-user statistics.</pre>



<pre>+--------------------------------+------------+</pre>



<pre>| Elapsed Time (Item Statistics) | % Complete |</pre>



<pre>+--------------------------------+------------+</pre>



<pre>| 79.562ms                       | 4.5        |</pre>



<pre>| 122.585ms                      | 100        |</pre>



<pre>+--------------------------------+------------+</pre>



<pre>Setting up lookup tables.</pre>



<pre>Processing data in one pass using dense lookup tables.</pre>



<pre>+-------------------------------------+------------------+-----------------+</pre>



<pre>| Elapsed Time (Constructing Lookups) | Total % Complete | Items Processed |</pre>



<pre>+-------------------------------------+------------------+-----------------+</pre>



<pre>| 450.626ms                           | 0                | 0               |</pre>



<pre>| 2.49s                               | 100              | 9952            |</pre>



<pre>+-------------------------------------+------------------+-----------------+</pre>



<pre>Finalizing lookup tables.</pre>



<pre>Generating candidate set for working with new users.</pre>



<pre>Finished training in 3.71243s</pre>


** Applying the personalized model to make some recommendations **


```python
personalized_model.recommend(users=[users[1]])
```




<div style="max-height:1000px;max-width:1500px;overflow:auto;"><table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">user_id</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">song</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">score</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">rank</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">279292bb36dbfc7f505e36ebf<br>038c81eb1d1d63e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Riot In Cell Block Number<br>Nine - Dr Feelgood ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0374999940395</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">279292bb36dbfc7f505e36ebf<br>038c81eb1d1d63e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Sei Lá Mangueira -<br>Elizeth Cardoso ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0331632643938</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">279292bb36dbfc7f505e36ebf<br>038c81eb1d1d63e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">The Stallion - Ween</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0322580635548</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">279292bb36dbfc7f505e36ebf<br>038c81eb1d1d63e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Rain - Subhumans</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0314159244299</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">4</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">279292bb36dbfc7f505e36ebf<br>038c81eb1d1d63e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">West One (Shine On Me) -<br>The Ruts ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0306771993637</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">5</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">279292bb36dbfc7f505e36ebf<br>038c81eb1d1d63e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Back Against The Wall -<br>Cage The Elephant ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0301204770803</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">6</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">279292bb36dbfc7f505e36ebf<br>038c81eb1d1d63e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Life Less Frightening -<br>Rise Against ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0284431129694</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">7</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">279292bb36dbfc7f505e36ebf<br>038c81eb1d1d63e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">A Beggar On A Beach Of<br>Gold - Mike And The ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0230024904013</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">8</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">279292bb36dbfc7f505e36ebf<br>038c81eb1d1d63e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Audience Of One - Rise<br>Against ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0193938463926</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">9</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">279292bb36dbfc7f505e36ebf<br>038c81eb1d1d63e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Blame It On The Boogie -<br>The Jacksons ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0189873427153</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">10</td>
    </tr>
</table>
[10 rows x 4 columns]<br/>
</div>




```python
personalized_model.get_similar_items(['With Or Without You - U2'])
```




<div style="max-height:1000px;max-width:1500px;overflow:auto;"><table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">song</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">similar</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">score</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">rank</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">With Or Without You - U2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">I Still Haven't Found<br>What I'm Looking For  ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.042857170105</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">With Or Without You - U2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Hold Me_ Thrill Me_ Kiss<br>Me_ Kill Me - U2 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0337349176407</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">With Or Without You - U2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Window In The Skies - U2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0328358411789</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">With Or Without You - U2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Vertigo - U2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0300751924515</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">4</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">With Or Without You - U2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Sunday Bloody Sunday - U2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0271317958832</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">5</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">With Or Without You - U2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Bad - U2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0251798629761</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">6</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">With Or Without You - U2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">A Day Without Me - U2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0237154364586</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">7</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">With Or Without You - U2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Another Time Another<br>Place - U2 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0203251838684</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">8</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">With Or Without You - U2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Walk On - U2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0202020406723</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">9</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">With Or Without You - U2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Get On Your Boots - U2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0196850299835</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">10</td>
    </tr>
</table>
[10 rows x 4 columns]<br/>
</div>




```python
personalized_model.get_similar_items(["Chan Chan (Live) - Buena Vista Social Club"])
```




<div style="max-height:1000px;max-width:1500px;overflow:auto;"><table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">song</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">similar</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">score</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">rank</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Chan Chan (Live) - Buena<br>Vista Social Club ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Murmullo - Buena Vista<br>Social Club ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.188118815422</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Chan Chan (Live) - Buena<br>Vista Social Club ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">La Bayamesa - Buena Vista<br>Social Club ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.18719214201</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Chan Chan (Live) - Buena<br>Vista Social Club ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Amor de Loca Juventud -<br>Buena Vista Social Club ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.184834122658</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Chan Chan (Live) - Buena<br>Vista Social Club ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Diferente - Gotan Project</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0214592218399</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">4</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Chan Chan (Live) - Buena<br>Vista Social Club ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Mistica - Orishas</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0205761194229</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">5</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Chan Chan (Live) - Buena<br>Vista Social Club ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Hotel California - Gipsy<br>Kings ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0193049907684</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">6</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Chan Chan (Live) - Buena<br>Vista Social Club ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Nací Orishas - Orishas</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0191571116447</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">7</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Chan Chan (Live) - Buena<br>Vista Social Club ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Gitana - Willie Colon</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.018796980381</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">8</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Chan Chan (Live) - Buena<br>Vista Social Club ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Le Moulin - Yann Tiersen</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.018796980381</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">9</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Chan Chan (Live) - Buena<br>Vista Social Club ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Criminal - Gotan Project</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.0187793374062</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">10</td>
    </tr>
</table>
[10 rows x 4 columns]<br/>
</div>



# Quantitative comparison between the models



```python
%matplotlib inline
model_performance = graphlab.recommender.util.compare_models(test_data,[popularity_model,personalized_model],user_sample=0.05)
```

    compare_models: using 2931 users to estimate model performance
    PROGRESS: Evaluate model M0
    


<pre>recommendations finished on 1000/2931 queries. users per second: 5190.6</pre>



<pre>recommendations finished on 2000/2931 queries. users per second: 5843.48</pre>


    
    Precision and recall summary statistics by cutoff
    +--------+-----------------+------------------+
    | cutoff |  mean_precision |   mean_recall    |
    +--------+-----------------+------------------+
    |   1    | 0.0283179802115 | 0.00799188964491 |
    |   2    | 0.0259297168202 | 0.0145291429068  |
    |   3    | 0.0238826339133 | 0.0193384376342  |
    |   4    | 0.0220914363698 | 0.0237693160928  |
    |   5    | 0.0197202320027 | 0.0263093616712  |
    |   6    | 0.0187649266462 | 0.0301699765861  |
    |   7    | 0.0183750060925 | 0.0343968360074  |
    |   8    | 0.0174854998294 | 0.0371247906033  |
    |   9    | 0.0167557526821 | 0.0402738090287  |
    |   10   | 0.0161719549642 | 0.0434842903098  |
    +--------+-----------------+------------------+
    [10 rows x 3 columns]
    
    PROGRESS: Evaluate model M1
    


<pre>recommendations finished on 1000/2931 queries. users per second: 5965.88</pre>



<pre>recommendations finished on 2000/2931 queries. users per second: 6364.89</pre>


    
    Precision and recall summary statistics by cutoff
    +--------+-----------------+-----------------+
    | cutoff |  mean_precision |   mean_recall   |
    +--------+-----------------+-----------------+
    |   1    |  0.191402251791 | 0.0611994441048 |
    |   2    |  0.161890139884 | 0.0978019513061 |
    |   3    |  0.142158535198 |  0.124678263136 |
    |   4    |  0.126407369498 |  0.144168821291 |
    |   5    |  0.114227226203 |  0.160200044179 |
    |   6    |  0.10440122825  |  0.175092411374 |
    |   7    | 0.0969927377297 |  0.188619806682 |
    |   8    | 0.0906687137496 |  0.202469948731 |
    |   9    | 0.0851434853482 |  0.214201103963 |
    |   10   | 0.0800068236097 |  0.223399851877 |
    +--------+-----------------+-----------------+
    [10 rows x 3 columns]
    
    


```python
model_performance
```




    [{'precision_recall_by_user': Columns:
      	user_id	str
      	cutoff	int
      	precision	float
      	recall	float
      	count	int
      
      Rows: 52758
      
      Data:
      +-------------------------------+--------+-----------+--------+-------+
      |            user_id            | cutoff | precision | recall | count |
      +-------------------------------+--------+-----------+--------+-------+
      | 0021d9a4628624f6d70237f9c2... |   1    |    0.0    |  0.0   |   9   |
      | 0021d9a4628624f6d70237f9c2... |   2    |    0.0    |  0.0   |   9   |
      | 0021d9a4628624f6d70237f9c2... |   3    |    0.0    |  0.0   |   9   |
      | 0021d9a4628624f6d70237f9c2... |   4    |    0.0    |  0.0   |   9   |
      | 0021d9a4628624f6d70237f9c2... |   5    |    0.0    |  0.0   |   9   |
      | 0021d9a4628624f6d70237f9c2... |   6    |    0.0    |  0.0   |   9   |
      | 0021d9a4628624f6d70237f9c2... |   7    |    0.0    |  0.0   |   9   |
      | 0021d9a4628624f6d70237f9c2... |   8    |    0.0    |  0.0   |   9   |
      | 0021d9a4628624f6d70237f9c2... |   9    |    0.0    |  0.0   |   9   |
      | 0021d9a4628624f6d70237f9c2... |   10   |    0.0    |  0.0   |   9   |
      +-------------------------------+--------+-----------+--------+-------+
      [52758 rows x 5 columns]
      Note: Only the head of the SFrame is printed.
      You can use print_rows(num_rows=m, num_columns=n) to print more rows and columns.,
      'precision_recall_overall': Columns:
      	cutoff	int
      	precision	float
      	recall	float
      
      Rows: 18
      
      Data:
      +--------+-----------------+------------------+
      | cutoff |    precision    |      recall      |
      +--------+-----------------+------------------+
      |   1    | 0.0283179802115 | 0.00799188964491 |
      |   2    | 0.0259297168202 | 0.0145291429068  |
      |   3    | 0.0238826339133 | 0.0193384376342  |
      |   4    | 0.0220914363698 | 0.0237693160928  |
      |   5    | 0.0197202320027 | 0.0263093616712  |
      |   6    | 0.0187649266462 | 0.0301699765861  |
      |   7    | 0.0183750060925 | 0.0343968360074  |
      |   8    | 0.0174854998294 | 0.0371247906033  |
      |   9    | 0.0167557526821 | 0.0402738090287  |
      |   10   | 0.0161719549642 | 0.0434842903098  |
      +--------+-----------------+------------------+
      [18 rows x 3 columns]
      Note: Only the head of the SFrame is printed.
      You can use print_rows(num_rows=m, num_columns=n) to print more rows and columns.},
     {'precision_recall_by_user': Columns:
      	user_id	str
      	cutoff	int
      	precision	float
      	recall	float
      	count	int
      
      Rows: 52758
      
      Data:
      +-------------------------------+--------+-----------+--------+-------+
      |            user_id            | cutoff | precision | recall | count |
      +-------------------------------+--------+-----------+--------+-------+
      | 0021d9a4628624f6d70237f9c2... |   1    |    0.0    |  0.0   |   9   |
      | 0021d9a4628624f6d70237f9c2... |   2    |    0.0    |  0.0   |   9   |
      | 0021d9a4628624f6d70237f9c2... |   3    |    0.0    |  0.0   |   9   |
      | 0021d9a4628624f6d70237f9c2... |   4    |    0.0    |  0.0   |   9   |
      | 0021d9a4628624f6d70237f9c2... |   5    |    0.0    |  0.0   |   9   |
      | 0021d9a4628624f6d70237f9c2... |   6    |    0.0    |  0.0   |   9   |
      | 0021d9a4628624f6d70237f9c2... |   7    |    0.0    |  0.0   |   9   |
      | 0021d9a4628624f6d70237f9c2... |   8    |    0.0    |  0.0   |   9   |
      | 0021d9a4628624f6d70237f9c2... |   9    |    0.0    |  0.0   |   9   |
      | 0021d9a4628624f6d70237f9c2... |   10   |    0.0    |  0.0   |   9   |
      +-------------------------------+--------+-----------+--------+-------+
      [52758 rows x 5 columns]
      Note: Only the head of the SFrame is printed.
      You can use print_rows(num_rows=m, num_columns=n) to print more rows and columns.,
      'precision_recall_overall': Columns:
      	cutoff	int
      	precision	float
      	recall	float
      
      Rows: 18
      
      Data:
      +--------+-----------------+-----------------+
      | cutoff |    precision    |      recall     |
      +--------+-----------------+-----------------+
      |   1    |  0.191402251791 | 0.0611994441048 |
      |   2    |  0.161890139884 | 0.0978019513061 |
      |   3    |  0.142158535198 |  0.124678263136 |
      |   4    |  0.126407369498 |  0.144168821291 |
      |   5    |  0.114227226203 |  0.160200044179 |
      |   6    |  0.10440122825  |  0.175092411374 |
      |   7    | 0.0969927377297 |  0.188619806682 |
      |   8    | 0.0906687137496 |  0.202469948731 |
      |   9    | 0.0851434853482 |  0.214201103963 |
      |   10   | 0.0800068236097 |  0.223399851877 |
      +--------+-----------------+-----------------+
      [18 rows x 3 columns]
      Note: Only the head of the SFrame is printed.
      You can use print_rows(num_rows=m, num_columns=n) to print more rows and columns.}]




```python
if graphlab.version[:3] >= "1.6":
    model_performance = graphlab.compare(test_data, [popularity_model, personalized_model], user_sample=0.05)
    graphlab.show_comparison(model_performance,[popularity_model, personalized_model])
else:
    %matplotlib inline
    model_performance = graphlab.recommender.util.compare_models(test_data, [popularity_model, personalized_model], user_sample=.05)
```

    compare_models: using 2931 users to estimate model performance
    PROGRESS: Evaluate model M0
    


<pre>recommendations finished on 1000/2931 queries. users per second: 6574.32</pre>



<pre>recommendations finished on 2000/2931 queries. users per second: 7000.28</pre>


    
    Precision and recall summary statistics by cutoff
    +--------+-----------------+------------------+
    | cutoff |  mean_precision |   mean_recall    |
    +--------+-----------------+------------------+
    |   1    | 0.0283179802115 | 0.00670208946821 |
    |   2    | 0.0264414875469 | 0.0132202613907  |
    |   3    | 0.0247924485386 | 0.0190076153612  |
    |   4    | 0.0217502558854 | 0.0218975199144  |
    |   5    | 0.0202661207779 | 0.0251390176032  |
    |   6    | 0.0197316046855 | 0.0303122343936  |
    |   7    | 0.0190573670615 | 0.0338003986801  |
    |   8    | 0.0185090412828 | 0.0370645063455  |
    |   9    | 0.0175139315364 | 0.0395220681173  |
    |   10   | 0.0164107813033 | 0.0410892090396  |
    +--------+-----------------+------------------+
    [10 rows x 3 columns]
    
    PROGRESS: Evaluate model M1
    


<pre>recommendations finished on 1000/2931 queries. users per second: 5372.56</pre>



<pre>recommendations finished on 2000/2931 queries. users per second: 5843.84</pre>


    
    Precision and recall summary statistics by cutoff
    +--------+-----------------+-----------------+
    | cutoff |  mean_precision |   mean_recall   |
    +--------+-----------------+-----------------+
    |   1    |  0.189696349369 | 0.0591444228087 |
    |   2    |  0.153872398499 | 0.0898157843142 |
    |   3    |  0.135562379165 |  0.114205917097 |
    |   4    |  0.123592630502 |  0.137573528579 |
    |   5    |  0.111634254521 |  0.153328818912 |
    |   6    |  0.101671784374 |  0.167400888281 |
    |   7    | 0.0950918750305 |  0.183292087002 |
    |   8    | 0.0892186966905 |  0.195791011464 |
    |   9    | 0.0845748512074 |  0.208899004536 |
    |   10   | 0.0802115319004 |  0.21907102621  |
    +--------+-----------------+-----------------+
    [10 rows x 3 columns]
    
    Model compare metric: precision_recall
    




```python
graphlab.show_comparison(model_performance,[popularity_model, personalized_model])
```




```python

```


```python
#users = song_data["user_id"].unique()
kanye_west = songs_data[songs_data["artist"] == "Kanye West"]
```


```python
kanye_west.head()
```




<div style="max-height:1000px;max-width:1500px;overflow:auto;"><table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">user_id</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">song_id</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">listen_count</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">title</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">artist</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">b80344d063b5ccb3212f76538<br>f3d9e43d87dca9e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOBXHDL12A81C204C0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Stronger</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Kanye West</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">b80344d063b5ccb3212f76538<br>f3d9e43d87dca9e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOMLMKI12A81C204BC</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Champion</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Kanye West</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">5d5e0142e54c3bb7b69f548c2<br>ee55066c90700eb ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SORFASW12A81C22AE7</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Stronger</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Kanye West</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">537340ff896dea11328910013<br>cfe759413e1eeb3 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOBXHDL12A81C204C0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Stronger</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Kanye West</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">7dd192c8bd4f27f573cb15e86<br>56442aadd7a9c01 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOOLPFK12A58A7BDE3</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">5</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Flashing Lights</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Kanye West</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">8fce200f3912e9608e3b1463c<br>db9c3529aab5c08 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOBXHDL12A81C204C0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Stronger</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Kanye West</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">8fce200f3912e9608e3b1463c<br>db9c3529aab5c08 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOIBSWV12A6D4F6AB3</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Through The Wire</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Kanye West</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">a56bf59af6edc5ae6c92d61dd<br>d214989332864e8 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SONGNHO12AB0183915</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Bad News</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Kanye West</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">8fa25e588aeedaa539674babb<br>75729ac9f31f15e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOOLPFK12A58A7BDE3</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Flashing Lights</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Kanye West</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">e8612acfb1572297ea0eaaa1f<br>27927d55fdcec65 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOIYWPZ12A81C204EF</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Homecoming</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Kanye West</td>
    </tr>
</table>
<table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">song</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Stronger - Kanye West</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Champion - Kanye West</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Stronger - Kanye West</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Stronger - Kanye West</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Flashing Lights - Kanye<br>West ...</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Stronger - Kanye West</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Through The Wire - Kanye<br>West ...</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Bad News - Kanye West</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Flashing Lights - Kanye<br>West ...</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Homecoming - Kanye West</td>
    </tr>
</table>
[10 rows x 6 columns]<br/>
</div>




```python
Akon = songs_data[songs_data["artist"] == "Akon"]
```


```python
Akon.head()
```




<div style="max-height:1000px;max-width:1500px;overflow:auto;"><table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">user_id</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">song_id</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">listen_count</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">title</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">artist</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">song</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">37029a65b9925fb3c1964774f<br>cab695b82955f76 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOILRPD12A6701E38C</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">9</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Locked Up</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Akon</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Locked Up - Akon</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">9a0215ffab0f8322aeb36fa66<br>36444b050462a51 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOGOAJS12A58A7A71F</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Freedom</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Akon</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Freedom - Akon</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">5e3d8c9728270d6557453f0c7<br>38e7f443feef13e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOCSUOL12AAA8C6707</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">5</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">The Rain</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Akon</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">The Rain - Akon</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1a640060dad45f7cb120578b2<br>4533f36907ad1b2 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOCSUOL12AAA8C6707</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">10</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">The Rain</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Akon</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">The Rain - Akon</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1b96dd04ce96aa7995e0f817e<br>762ca44a24aab24 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOILRPD12A6701E38C</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Locked Up</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Akon</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Locked Up - Akon</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">c231bc806c239b1322421e66f<br>c001822a9b2c2f0 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOGOAJS12A58A7A71F</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Freedom</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Akon</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Freedom - Akon</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">c78aa603dd64a81573f6050ed<br>f759bbc43bf7776 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOAMEVJ12A6701E393</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Lonely</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Akon</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Lonely - Akon</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">38d0e8b1fb8fd58a92a0eae64<br>80a00a02d51eb65 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOAMEVJ12A6701E393</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Lonely</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Akon</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Lonely - Akon</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">d4c9f015332918148fec23033<br>1d50e34d3c4c27b ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOWONPQ12A8BED02A8</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Don't Matter</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Akon</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Don't Matter - Akon</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">b060d8ee0a018bc167f2feaaf<br>9f50d5c84ac6ae4 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOWONPQ12A8BED02A8</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Don't Matter</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Akon</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Don't Matter - Akon</td>
    </tr>
</table>
[10 rows x 6 columns]<br/>
</div>




```python
foo_fighters = songs_data[songs_data["artist"] == "Foo Fighters"]
```


```python
foo_fighters.head()
```




<div style="max-height:1000px;max-width:1500px;overflow:auto;"><table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">user_id</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">song_id</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">listen_count</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">title</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">artist</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">b80344d063b5ccb3212f76538<br>f3d9e43d87dca9e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SODACBL12A8C13C273</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Learn To Fly</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Foo Fighters</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">b80344d063b5ccb3212f76538<br>f3d9e43d87dca9e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOFGUAY12AB017B0A8</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Stacked Actors</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Foo Fighters</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">b80344d063b5ccb3212f76538<br>f3d9e43d87dca9e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOMSQJY12A8C138539</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Breakout</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Foo Fighters</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">b80344d063b5ccb3212f76538<br>f3d9e43d87dca9e ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOVHRGF12A8C13852F</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Generator</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Foo Fighters</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">12768858f6a825452e412deb1<br>df36d2d1d9c6791 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SODACBL12A8C13C273</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">4</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Learn To Fly</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Foo Fighters</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">12768858f6a825452e412deb1<br>df36d2d1d9c6791 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOQLUTQ12A8AE48037</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">The Pretender</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Foo Fighters</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">f47116f998e030f2dab275b81<br>fb2a04a9dc06c33 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOLTAEJ12A8C13F793</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">6</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">What If I Do?</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Foo Fighters</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">5e161b9e14f303a0cef2d3f44<br>d07dd946549f89f ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOLJYEI12A8C13F7B3</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Friend Of A Friend</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Foo Fighters</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">e4c05157f8cebdf3b9d689c44<br>1ba97c5ed5db05b ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOFGUAY12AB017B0A8</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Stacked Actors</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Foo Fighters</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">e4c05157f8cebdf3b9d689c44<br>1ba97c5ed5db05b ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOFZVOT12A8C1408E9</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Skin And Bones</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Foo Fighters</td>
    </tr>
</table>
<table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">song</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Learn To Fly - Foo<br>Fighters ...</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Stacked Actors - Foo<br>Fighters ...</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Breakout - Foo Fighters</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Generator - Foo Fighters</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Learn To Fly - Foo<br>Fighters ...</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">The Pretender - Foo<br>Fighters ...</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">What If I Do? - Foo<br>Fighters ...</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Friend Of A Friend - Foo<br>Fighters ...</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Stacked Actors - Foo<br>Fighters ...</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Skin And Bones - Foo<br>Fighters ...</td>
    </tr>
</table>
[10 rows x 6 columns]<br/>
</div>




```python
foof_fighters_users = foo_fighters["user_id"].unique()
```


```python
len(foof_fighters_users)
```




    2055




```python
taylor_swift = songs_data[songs_data["artist"] == "Taylor Swift"]
```


```python
taylor_swift.head()
```




<div style="max-height:1000px;max-width:1500px;overflow:auto;"><table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">user_id</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">song_id</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">listen_count</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">title</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">artist</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">169f9f4c68b62d1887c7c0ac9<br>9d10a79cfca5daf ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOCLMAD12AB017FC09</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Tim McGraw</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Taylor Swift</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">81bde1c3a845c64f1677bd9d2<br>8f2da85dfefcf30 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOTWSXL12A8C143349</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Love Story</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Taylor Swift</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0152fcbd02b172a874c75a57a<br>913f0f0109ba272 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOSAXUZ12AAF3B2031</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">The Best Day</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Taylor Swift</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0152fcbd02b172a874c75a57a<br>913f0f0109ba272 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOSROFB12AAF3B4C5D</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">You Belong With Me</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Taylor Swift</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">8cbb5066924ec788e3fea9a4a<br>ae59586f46f38fa ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOTWSXL12A8C143349</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Love Story</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Taylor Swift</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">ea07020bb223c733ccc55aa92<br>5ebcc25c4d97377 ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOMPTCI12AB017C416</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">13</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Forever &amp; Always</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Taylor Swift</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">85d0d381551960608e02df989<br>56277e495b3cf6b ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOSROFB12AAF3B4C5D</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">You Belong With Me</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Taylor Swift</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">5d5e0142e54c3bb7b69f548c2<br>ee55066c90700eb ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOLJTMU12AAF3B4C4D</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Hey Stephen</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Taylor Swift</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">5d5e0142e54c3bb7b69f548c2<br>ee55066c90700eb ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SORRBVQ12A58A7AA33</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Change</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Taylor Swift</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">5d5e0142e54c3bb7b69f548c2<br>ee55066c90700eb ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">SOTNWCI12AAF3B2028</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Forever &amp; Always</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Taylor Swift</td>
    </tr>
</table>
<table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">song</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Tim McGraw - Taylor Swift</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Love Story - Taylor Swift</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">The Best Day - Taylor<br>Swift ...</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">You Belong With Me -<br>Taylor Swift ...</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Love Story - Taylor Swift</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Forever &amp; Always - Taylor<br>Swift ...</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">You Belong With Me -<br>Taylor Swift ...</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Hey Stephen - Taylor<br>Swift ...</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Change - Taylor Swift</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Forever &amp; Always - Taylor<br>Swift ...</td>
    </tr>
</table>
[10 rows x 6 columns]<br/>
</div>




```python
# Total No. of Artists
total_count = songs_data.groupby(key_columns = "artist", operations={"total_count":graphlab.aggregate.SUM("listen_count")})
```


```python
total_count.head()
```




<div style="max-height:1000px;max-width:1500px;overflow:auto;"><table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">artist</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">total_count</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">The Dells</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">274</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Lil Jon / The East Side<br>Boyz ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">197</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Tom Petty And The<br>Heartbreakers ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2867</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Blackstreet</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">747</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Ratatat</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3727</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Shotta</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">82</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Airscape</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">130</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Mecano</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">172</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Moimir Papalescu &amp; The<br>Nihilists ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">177</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Brad Paisley</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2731</td>
    </tr>
</table>
[10 rows x 2 columns]<br/>
</div>




```python
total_count.sort("total_count",ascending=False)
```




<div style="max-height:1000px;max-width:1500px;overflow:auto;"><table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">artist</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">total_count</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Kings Of Leon</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">43218</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Dwight Yoakam</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">40619</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Björk</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">38889</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Coldplay</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">35362</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Florence + The Machine</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">33387</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Justin Bieber</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">29715</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Alliance Ethnik</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">26689</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">OneRepublic</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">25754</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Train</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">25402</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">The Black Keys</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">22184</td>
    </tr>
</table>
[3375 rows x 2 columns]<br/>Note: Only the head of the SFrame is printed.<br/>You can use print_rows(num_rows=m, num_columns=n) to print more rows and columns.
</div>




```python
total_count.sort("total_count",ascending=True)
```




<div style="max-height:1000px;max-width:1500px;overflow:auto;"><table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">artist</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">total_count</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">William Tabbert</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">14</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Reel Feelings</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">24</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Beyoncé feat. Bun B and<br>Slim Thug ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">26</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Diplo</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">30</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Boggle Karaoke</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">30</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">harvey summers</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">31</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Nâdiya</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">36</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Kanye West / Talib Kweli<br>/ Q-Tip / Common / ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">38</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Aneta Langerova</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">38</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Jody Bernal</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">38</td>
    </tr>
</table>
[3375 rows x 2 columns]<br/>Note: Only the head of the SFrame is printed.<br/>You can use print_rows(num_rows=m, num_columns=n) to print more rows and columns.
</div>




```python

```
