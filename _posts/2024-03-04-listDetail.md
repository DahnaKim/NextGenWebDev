---
layout: post
title:  "Modify File Upload, Implement List and Detail Pages"
categories: [ WebDev ]
image: assets/img/20240304_1.png
tags: [featured]
---
  
# Modify File Upload, Implement List and Detail Pages  
  
### 파일 업로드 데이터에 제목, 설명 기능 넣기  
  
**sql**  
~~~
ALTER TABLE images  
ADD COLUMN title VARCHAR(255),  
ADD COLUMN description TEXT;  
~~~

<br>

**html**  
~~~
<body>  
<h1>Upload File</h1>  
    <form action="/upload" method="post" enctype="multipart/form-data">  
        <input type="text" name="title" placeholder="Title" required>  
        <textarea name="description" placeholder="Description" required></textarea>  
        <input type="file" name="file" required>  
        <input type="submit" value="Upload">  
    </form>  
</body>  
~~~

<br>

**main.py**  
![1]({{ site.baseurl }}/assets/img/20240304_1.png)  

<br>

### 리스트 페이지  
**main.py**  
~~~
def listall():  
    cursor.execute("SELECT id, title, file_path FROM images")  
    images = cursor.fetchall()  
    return render_template('list.html', images=images)  
~~~

<br>

**list.html**  
~~~
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Select Artwork</title>  
    <style>  
        img {  
            width: 200px; /* 이미지 크기 조정 */  
            height: auto;  
        }  
    </style>  
</head>  
<body>  
<h1>Select Artwork</h1>  
<ul>
    {% raw %}
    {% for image in images %}  
    <li>  
        <a href="/listall/{{ image[0] }}">{{ image[1] }}</a>  
        <img src="/{{ image[2] }}" alt="{{ image[1] }}"><!-- 이미지 표시 -->  
    </li>  
    {% endfor %}
    {% endraw %}  
</ul>  
</body>  
</html>  
~~~  

![2]({{ site.baseurl }}/assets/img/20240304_2.png)
![3]({{ site.baseurl }}/assets/img/20240304_3.png)

<br>

### 상세페이지  
  
**main.py**  
~~~
@app.route('/view_artwork/<int:artwork_id>')
def view_artwork(artwork_id):
    cursor.execute("SELECT id, title, description, file_path, created_at FROM images WHERE id = %s", (artwork_id,))
    artwork = cursor.fetchone()
    if artwork:
        return render_template('view_artwork.html', artwork=artwork)
    else:
        return "Artwork not found", 404
~~~

<br>

**list.html 수정**
~~~
<body>
<h1>Select Artwork</h1>
<ul>
    {% raw %}
    {% for image in images %}
    <li>
        <!-- 작품 상세 페이지로 이동하는 링크 업데이트 -->
        <a href="/view_artwork/{{ image[0] }}">{{ image[1] }}</a>
        <!-- 이미지 경로 생성을 Flask의 url_for 함수를 사용하도록 수정 -->
        <img src="{{ url_for('static', filename=image[2][7:]) }}" alt="{{ image[1] }}" style="width:200px; height:auto;">
    </li>
    {% endfor %}
    {% endraw %}
</ul>
</body>
~~~

<br>

**view_artwork.html**  
~~~
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>View Artwork</title>
</head>
<body>
    <h1>{{ artwork[1] }}</h1>
    <img src="{{ url_for('static', filename='uploads/' + artwork[3].split('/')[-1]) }}" alt="{{ artwork[1] }}" style="width:100%;max-width:600px;">
    <p>Created at: {{ artwork[4] }}</p>
    <p>{{ artwork[2] }}</p>
    <a href="/listall">Back to list</a>
</body>
</html>
~~~

![4]({{ site.baseurl }}/assets/img/20240304_4.png)


