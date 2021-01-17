---
title: "Flutter Part 3: State"
date: 2019-12-11 15:22:41
hidden: false
draft: false
tags: ["flutter","App"]
slug: "Flutter 3"
---
## State & HTTP request

**Dart and Flutter: The Complete Developer's Guide**: Chapter 9~11
Let's learn more about flutter widget.
Main Goal: Click the action button to request one more image
Also, play with material wedgets. 🌟

<!--more-->

### Stateless / Stateful Widget

**Stateless**: no interaction , eg. Icon, Text...

```dart
class MyApp extends StatelessWidget {
  Widget build(BuildContext context) {
    return MaterialApp(...);
  }
}
```

---------------

**Stateful**: interaciton with user, eg. Form, Checkbox, Slider...

State could be update by user and saved.

```dart
class App extends StatefulWidget {
  createState() {
    return AppState();
  }
}

class AppState extends State<App> {
  Widget build(BuildContext context) {
    return (...)
  }
}
```

### HTTP request: GET

 1. Import package

```dart
import 'package:http/http.dart' show get;
```

 2. Get response

```dart
var response = get('https://jsonplaceholder.typicode.com/posts/1');
```

 3. `async` or not
if request as **await**, need to be marked as **async**


```dart
fetchImage() async {
 var response = await get('https://jsonplaceholder.typicode.com/post/1');
}
```

💡[Dart: futures, `async`, `await`](https://dart.dev/codelabs/async-await)

### Photo Request Example

#### Model

```dart
class ImageModel {
  int id;
  String url;
  String title;

  ImageModel(this.id, this.url, this.title);

  ImageModel.fromJson(Map<String, dynamic> parsedJson) {
    id = parsedJson['id'];
    url = parsedJson['url'];
    title = parsedJson['title'];
  }
}
```

##### shorthand

```dart
ImageModel.fromJson(Map<String, dynamic> parsedJson)
  : id = parsedJson['id'],
    url = parsedJson['url'],
    title = parsedJson['title'];
```

#### Widget

```dart
import 'package:flutter/material.dart';
import '../models/image_model.dart';

class ImageList extends StatelessWidget {
  final List<ImageModel> images;

  ImageList(this.images);

  Widget build(context) {
    return ListView.builder(
      itemCount: images.length,
      itemBuilder: (context, int index) {
        return buildImage(images[index]);
      }
    );
  }

  Widget buildImage(ImageModel image) {
    return Container(
      margin: EdgeInsets.fromLTRB(16.0, 8.0, 16.0, 8.0),
      padding: EdgeInsets.all(6.0),
      decoration: BoxDecoration(border: Border.all(width: 3, color: Colors.blueGrey)),
      child: Column(
        children: <Widget>[
          Image.network(image.url),
          Padding(
            child: Text(image.title),
            padding: EdgeInsets.all(10.0)
          )
        ],
      ),
    );
  }
}
```

#### App

```dart
import 'package:flutter/material.dart';
import 'dart:convert';
import 'package:http/http.dart' show get;
import 'models/image_model.dart';
import 'widgets/image_list.dart';

class App extends StatefulWidget {
  createState() {
    return AppState();
  }
}

class AppState extends State<App> {
  int counter = 0;
  List<ImageModel> images = [];

  void fetchImage() async {
    counter++;
    var res = await get('https://jsonplaceholder.typicode.com/photos/$counter');
    var imageModel = ImageModel.fromJson(json.decode(res.body));

    setState(() {
      images.add(imageModel);
    });
  }

  Widget build(context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('Hello'),
        ),
        body: ImageList(images),
        floatingActionButton: FloatingActionButton(
          child: Icon(Icons.add),
          onPressed: fetchImage,
        ),
      ),
    );
  }
}
```

#### main

```dart
import 'package:flutter/material.dart';
import 'src/app.dart';

void main() {
  runApp(App());
}
```