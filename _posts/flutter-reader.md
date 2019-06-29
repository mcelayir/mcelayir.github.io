---
layout: post
title:  "Getting Started With Flutter"
date:   2019-05-31
categories: posts
---

# What is flutter

Flutter is a free and open-source mobile application development SDK for building high-performance, native-looking apps for IOS and Android sharing the same codebase. Flutter aims to make development process easier, quick and more productive.

# What makes flutter cool

Flutter does not use Javascript but uses Dart, a simple object-oriented programming language. Since it is compiled into binary code, Dart can run like a native app and communicate with the platform without using a Javascript bridge. That's why it is more performant then other cross platform development SDKs.

Flutter doesn't use native UI components. Instead Flutter, has its own UI components (they are called <a href="https://flutter.dev/docs/development/ui/widgets">widgets</a>) and an engine to render them on both Android and IOS platforms.

Also I would like to mention that Flutter has `Hot Reload` feature which allows developers to change the code and reload as the application runs.

Lastly, Flutter is a very useful tool for prototyping and creating mockups as it is very easy to design and create good looking UIs with Flutter.

# Installing flutter

To install flutter on your machine, simply check suitable <a href="https://flutter.dev/docs/get-started/install">installation guide</a> for your platform.

Flutter SDK can also be installed using official Intellij plugin.

# Installing Intellij plugin

To install the Intellij plugin for Flutter. Go to <i>Preferences > Plugins</i>, select <i>Browse Repositories</i> and search for <i>Flutter</i>. 

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/flutter/Screenshot+2019-06-26+at+22.06.01.png"/>

# Creating a new project

Now, with the Flutter pluggin installed, a template project can easily 
be created by selecting Flutter under <i> New Project</i>.

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/flutter/Screenshot+2019-06-26+at+22.13.31.png"/>

Also, if you have not yet installed the SDK, it can be downloaded via this menu.

# App overview

In this tutorial, we are going to build a simple JSON/XML reader application. 

First the app will read categories and sources (like BBC News or Goal.com) associated with those categories and display them for user to select.

Sources 1             |  Sources 2
:-------------------------:|:-------------------------:
![](https://s3.eu-central-1.amazonaws.com/tutorial.assets/flutter/Simulator+Screen+Shot+-+iPhone+X%CA%80+-+2019-06-26+at+22.51.16.png)  |  ![](https://s3.eu-central-1.amazonaws.com/tutorial.assets/flutter/Simulator+Screen+Shot+-+iPhone+X%CA%80+-+2019-06-26+at+23.04.42.png)


# Code organization

Main.dart bahset

# Adding dependencies
In this tutorial we are going to make http requests, read rss files and show content inside a webview. To do that, we are going to need following packages.

<ul>
    <li><i>http</i> to make http requests</li>
    <li><i>webfeed</i> to parse rss feeds</li>
    <li><i>webview_flutter</i> to abe able to use webview component</li>
</ul>

You can add following lines to the `pubspec.yaml`file and hit the `Packages get` button in the top left of the window.

```yaml
  http: ^0.12.0+2
  webfeed: ^0.4.2
  webview_flutter: ^0.3.9+1
```

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/flutter/Screenshot+2019-06-28+at+22.30.09.png"/>

# Step 1 - Categories and Sources

## Defining assets

In this step we are going to build our first screen, where user will select the source to be readed.

So let's start with defining and loading the data. We are going to load 
categories and sources assigned to them from a JSON file.

Create a directory named `data` in your projects root directory and create `categories.json` file under it.

Our json data will be the following. Please minify the content in a <a href="https://codebeautify.org/jsonviewer">json modifier</a> before pasting it. There is a problem with the pretty printed json files while loading them which I couldn't figured out. If you have a solution for this, please reach out.

```json
[
  {
    "id": 1,
    "name": "news",
    "sources": [
      {
        "name": "BBC News",
        "link": "http://feeds.bbci.co.uk/news/rss.xml",
        "thumbnail": "https://pbs.twimg.com/profile_images/1001759983363641344/pJANPf_h_400x400.jpg"
      },
      {
        "name": "CNN",
        "link": "http://rss.cnn.com/rss/edition.rss",
        "thumbnail": "https://pbs.twimg.com/profile_images/508960761826131968/LnvhR8ED_400x400.png"
      }
    ]
  },
  {
    "id": 2,
    "name": "sports",
    "sources": [
      {
        "name": "NBA",
        "link": "http://feeds.feedburner.com/nba/PdXr",
        "thumbnail": "https://pbs.twimg.com/profile_images/921248739746033665/cjBVcCJG_400x400.jpg"
      },
      {
        "name": "ESPN FC",
        "link": "http://feeds.feedburner.com/espnfc/ziBg",
        "thumbnail": "https://pbs.twimg.com/profile_images/993576884498849792/zH7kViGI_400x400.jpg"
      },
      {
        "name": "Four Four Two",
        "link": "http://feeds.feedburner.com/fourfourtwo/qNct",
        "thumbnail": "https://the18.com/sites/default/files/FourFourTwo-Logo.jpg"
      }
    ]
  },
  {
    "id": 3,
    "name": "technology",
    "sources": [
      {
        "name": "BBC Technology",
        "link": "http://feeds.bbci.co.uk/news/technology/rss.xml",
        "thumbnail": "https://pbs.twimg.com/profile_images/1001759983363641344/pJANPf_h_400x400.jpg"
      },
      {
        "name": "CNN Technology",
        "link": "http://rss.cnn.com/rss/edition_technology.rss",
        "thumbnail": "https://pbs.twimg.com/profile_images/508960761826131968/LnvhR8ED_400x400.png"
      }
    ]
  }
]
```
To be able to access this file, we have to define it as an `asset`in `pubspec.yaml` file.

Define the file as following.

```yaml
flutter:

  assets:
    - data/categories.json
```

## Model

Our data consist of categories and their associated sources. We can create two classes to model our data. 

```dart
class Category{
  int id;
  String name;
  List<Source> sources;

  Category({this.id, this.name, this.sources});

  factory Category.fromJson(Map<String, dynamic> map){

    List<Source> _parseSources(List<dynamic> map){
      List<Source> list = map.map((item) => Source.fromJson(item)).toList();
      return list;
    }

    return Category(
        id: map['id'],
        name: map['name'],
        sources: _parseSources(map['sources'])
    );
  }

}

class Source {
  String name;
  String thumbnailUrl;
  String link;

  Source({this.name, this.thumbnailUrl, this.link});

  factory Source.fromJson(Map<String, dynamic> map){
    return Source(
        name: map['name'],
        thumbnailUrl: map['thumbnail'],
        link: map['link']
    );
  }
}
```

Notice that, we had to implement `factory <Classs>.fromJson(Map<String, dynamic> map)` to instruct the object, how to parse itself from json.

## Data Service

Now let's read load and parse the json file.

In `dart` there is no special keyword for interface, but a class can implement another class using `implements` keyword. The reason for that is, in `dart`, every class implicitly defines an interface containing all the instance members of the class and of any interfaces it implements.

Abstract classes can be defined using `abstract`keyword. An `abstract class` key either be `implemented`or `extended`.

Create an abstract class to define our data service API and the concrete class to load data from json file.

```dart
abstract class DataService{
  Future<List<Category>> getCategories();
}

class JsonDataService extends DataService{

  static final String filePath = 'data/categories.json';

  @override
  Future<List<Category>> getCategories() async{
    String content = await rootBundle.loadString(filePath);
    List<dynamic> parsedJson = json.decode(content);
    return parsedJson.map((value) => Category.fromJson(value)).toList();
  }

}
```

Let's walk over this code:
- `rootBundle` is used to access the file and content loaded with `loadString`method.
- To use asyncronious features, like `await`, a method should be marked as `async`. 
- `json.decode` method converts json string to a `Map` object.
- `map` method used to transform data into list of plain objects.

Now we have our data to display is ready. We can start implementing the interface.

## Information about widgets

It is safe to begin by saying, in Flutter, everything is a widget. Widgets are pieces of the user interface that are generally small and
reusable. Widgets are similar to views (or UIView) in Android and IOS.

There are two types of widgets:

### Stateless widgets

As their name indicates, this type of widgets doesn't hold any state.
An icon would be a great example for a stateless widget. An image is set for the icon during creating and it doesn't change. Also Text
widget is another example of a stateless widget. Text widget doesn't have a text property, that can be changed. To change the text value, you simply recreate a widget.

### Stateful widgets

A stateful widget comes with two classes: the widget itself and the state class. They are used to keep track of changes and update the UI based on those changes. When the values in the State object change,
a new widget is created. 

## Category header

We are going to create a simple widget to display our categories as headers.

```dart
Widget CategoryHeader(BuildContext context, Category category){

  return ListTile(
    title: Text(category.name),
  );
}
```

The widget takes a `BuildContext` and a `Category`and when called it will return a `ListTile` widget under the hood.

## Source card

For source card, we are going to return a `Card` widget.

```dart
Widget SourceCard(BuildContext context, Source source){

  return
    new Card(
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.center,
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            new Container(
                padding: const EdgeInsets.all(10.0),
                width:200.0,
                height: 200.0,
                decoration: new BoxDecoration(
                    shape: BoxShape.circle,
                    image: new DecorationImage(
                        fit: BoxFit.fill,
                        image: new NetworkImage(source.thumbnailUrl)
                    )
                )),
            SizedBox(height: 10),
            new Text(source.name,
              textScaleFactor: 1.5,
              style: new TextStyle(
                fontFamily: "Open Sans",
                fontSize: 20.0,
              ),
            )
          ],
        )
    );
}
```

## List view

Now that we created the blueprints to display our data. We can now implement the route (or layout whatever you want to say) to display them.

```dart
class CategoryRoute extends StatelessWidget {

  final DataService loader = new JsonDataService();

  List<Object> _flatten(Category category){
    final List<Object> result = new List();
    result.add(category);
    category.sources.forEach((item) => result.add(item));
    return result;
  }

  @override
  Widget build(BuildContext context) {

    return Scaffold(
      appBar: AppBar(title: Text('Sources')),
      body: Center(
      child:
      new FutureBuilder<List<Category>>(
          future: loader.getCategories(),
          builder: (context, snapshot) {
            if (snapshot.hasData) {
  
  
              List<Object> list = new List();
              snapshot.data.forEach((item) =>
                  list.addAll(_flatten(item))
              );
  
              return new Container(
                  padding: new EdgeInsets.all(20.0),
                  child: new ListView.separated(
                      itemCount: list.length,
                      itemBuilder: (context, index) {
  
                        Object item = list[index];
                        if(item is Category){
                          return CategoryHeader(context, item);
  
                        } else if(item is Source){
                          return
                            InkWell(
                                onTap: () {
                                  print('tapped');
                                  Navigator.push(
                                    context,
                                    MaterialPageRoute(builder: (context) => SourceFeedRoute(item)),
                                  );
                                },
                                child: SourceCard(context, item)
                            );
                        }
                      },
                      separatorBuilder: (context, index) {
                        return Divider();
                      })
              );
            } else if (snapshot.hasError) {
              return new Text("${snapshot.error}");
            }
            return new CircularProgressIndicator();
          },
        ),
      )
    );
  }
}
```
Things to mention here:
- The `Scaffold` is an important feature. In Flutter, `Scaffold` implements the basic material design visual layout structure. Multiple widgets can be composed with `Scaffold`.
- `FutureBuilder`builds itself based on the latestinteraction with a Future. The result of the interaction is obtained through `snapshot.data`.
- Since our has a nested structure, we used `_flatten` (`_`is for indicating private methods) method to create a list that contains both categories and sources as seperate objects.
- In Dart, type check can be done with `is` operator.
- `InkWell` widget is used to make the item clickable.
- `Center` is used to centerize its childs.

To make Flutter show this route on startup, we have to define it in `home` section of the main widget.

Change the `main.dart` with the following and run the application.

```dart
void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Reader',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: new CategoryRoute(),
    );
  }
}
```

Now, you should be able to see the app running successfully.

<img 
style="width:80%; height:80%"
src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/flutter/Simulator+Screen+Shot+-+iPhone+X%CA%80+-+2019-06-26+at+22.51.16.png"/>

# Step 2 - Feeds of sources

## Rss Reader Service

We should define a rss service to get rss files over http and parse them.

```dart
class RssService {

  Future<RssFeed> getFeed(String url) async{
    print("fetching $url ... ");
    var response = await get(url);
    print("fetched with code: ${response.statusCode}");
    return new RssFeed.parse(response.body);
  }
}
```

## Rss Item Card

The card will be the same as the source card. If the rss item has a thumbnail, it will be displayed. If not only the title will be displayed.

```dart
Widget SourceCard(BuildContext context, RssItem rssItem){

  String _getThumbUrl(){
    if(rssItem.media.thumbnails.length > 0 ){
      return rssItem.media.thumbnails[0].url;
    }else if (rssItem.enclosure != null && rssItem.enclosure.url != null && rssItem.enclosure.url != ""){
      return rssItem.enclosure.url;
    }
    return "";
  }

  double _getHeight(){
    String thumb = _getThumbUrl();
    if(thumb != "") return 300;
    return 0;
  }

  return
    new Card(
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.center,
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            new Container(
                padding: const EdgeInsets.all(10.0),
                height: _getHeight(),
                decoration: new BoxDecoration(
                    shape: BoxShape.rectangle,
                    image: new DecorationImage(
                        fit: BoxFit.fill,
                        image: new NetworkImage(_getThumbUrl())
                    )
                )),
            SizedBox(height: 10),
            new Text(rssItem.title,
              textScaleFactor: 1.5,
              style: new TextStyle(
                fontFamily: "Open Sans",
                fontSize: 20.0,
              ),
            )
          ],
        )
    );
}
```

## Displaying Feed Items.

Create another route to display rss feed items as list.

```dart
class SourceFeedRoute extends StatelessWidget {

  final RssService rssService = new RssService();
  final Source source;
  SourceFeedRoute(this.source);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text(source.name)),
      body: Center(
      child:
        new FutureBuilder<RssFeed>(
          future: rssService.getFeed(source.link),
          builder: (context, snapshot) {
            if (snapshot.hasData) {

              return new Container(
                  padding: new EdgeInsets.all(20.0),
                  child: new ListView.separated(
                      itemCount: snapshot.data.items.length,
                      itemBuilder: (context, index) {
                        RssItem item = snapshot.data.items[index];
                        return
                          InkWell(
                              onTap: () {
                                print('tapped');
                              },
                              child: RssItemCard(context, item)
                          );
                      },
                      separatorBuilder: (context, index) {
                        return Divider();
                      })
              );
            } else if (snapshot.hasError) {
              return new Text("${snapshot.error}");
            }
            return new CircularProgressIndicator();
        },
      ),
    )
    );
  }
}
```

# Step 3 - Navigation

To navigate between routes, we have to push them to navigator.

Change the `InkWell` widget in `CategoryRoute` to add navigation capability to `SourceFeedRoute`.

```dart
InkWell(
    onTap: () {
        print('tapped');
        Navigator.push(
            context,
            MaterialPageRoute(builder: (context) => SourceFeedRoute(item)),
        );
    },
    child: SourceCard(context, item)
);
```

Now select a source and navigate to its contents.

<img 
style="width:80%; height:80%"
src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/flutter/Simulator+Screen+Shot+-+iPhone+X%CA%80+-+2019-06-29+at+17.42.40.png">

<img 
style="width:80%; height:80%"
src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/flutter/Simulator+Screen+Shot+-+iPhone+X%CA%80+-+2019-06-29+at+17.42.48.png">

<img 
style="width:80%; height:80%"
src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/flutter/Simulator+Screen+Shot+-+iPhone+X%CA%80+-+2019-06-29+at+17.42.53.png">

# Step 5 - Web view

# Step 6 - Adding fonts

# Step 7 - styling

# Step 7 - Adding like and dislike buttons

# References
<ul>
<li><a href="https://flutter.dev/docs/resources/technical-overview">https://flutter.dev/docs/resources/technical-overview</a></li>

<li><a href="https://hackernoon.com/whats-revolutionary-about-flutter-946915b09514">https://hackernoon.com/whats-revolutionary-about-flutter-946915b09514</a></li>

<li><a href="https://blog.codemagic.io/what-is-flutter-benefits-and-limitations/">https://blog.codemagic.io/what-is-flutter-benefits-and-limitations/</a></li>

<li><a href="https://flutter.dev/docs/get-started/install"> https://flutter.dev/docs/get-started/install</a></li>

https://flutter.dev/docs/cookbook/networking/fetch-data

https://flutterbyexample.com/flutter-widgets/

https://pusher.com/tutorials/flutter-widgets
    
</ul>