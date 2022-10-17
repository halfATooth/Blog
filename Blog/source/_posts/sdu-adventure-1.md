---
title: 山大迷踪移动日志1
date: 2022-07-22 22:23:34
tags: 学线实训
---

# 对数据库sqflite的封装

因为要临时存储用户登录状态，存储token等等的需求，所以把基本的很繁琐的数据库操作封装到DBManager和DBProvider，两个类中。具体的数据库操作对象只需要继承DBProvider类，然后自定义增删改查的函数即可。

```dart
class DBManager{
  static const int _VERSION = 1;
  static const String _DB_NAME = 'isdu.db';
  static Database? _database;
  static init() async {
    var dbPath = await getDatabasesPath();
    String dbName = _DB_NAME;
    String path = dbPath + dbName;
    if(Platform.isIOS){
      path = dbPath + "/" + dbName;
    }
    _database = await openDatabase(
      path,
      onCreate: (Database db,int version) async {
      },
      version: _VERSION,
    );
  }
  static isTableExists(String tableName) async {
    await getCurrentDatabase();
    String sql =
        "select * from Sqlite_master where type = 'table' and name = '$tableName'";
    var res = await _database?.rawQuery(sql);
    return res != null && res.isNotEmpty;
  }
  static Future<Database?> getCurrentDatabase() async {
    if(_database == null) {
      await init();
    }
    return _database;
  }
  static close(){
    _database?.close();
    _database = null;
  }
}
abstract class DBProvider{
  bool isTableExits = false;
  tableSqlString();
  tableName();
  Future<Database> getDatabase() async {
    return await open();
  }
  @mustCallSuper
  prepare(name, String createSql) async {
    isTableExits = await DBManager.isTableExists(name);
    if(!isTableExits){
      Database? db = await DBManager.getCurrentDatabase();
      return await db?.execute(createSql);
    }
  }
  @mustCallSuper
  open() async {
    if(!isTableExits){
      await prepare(tableName(), tableSqlString());
    }
    return await DBManager.getCurrentDatabase();
  }
}
```

之后，使用DBProvider即可构建所需的dao。例：

```dart
class LoginDao extends DBProvider{
  final String name = "loginCheck";
  final String columnId = 'id';

  @override
  tableName() => name;

  @override
  tableSqlString() {
    return 'create table $name ($columnId integer primary key autoincrement,isLogin INTEGER)';
  }

  Future insert(LoginCheck loginCheck) async {
    Database db = await getDatabase();
    return await db.insert(name, loginCheck.toMap());
  }
  Future update(LoginCheck loginCheck) async {
    Database db = await getDatabase();
    return await db.update(name,loginCheck.toMap(),where: 'id = ?',whereArgs: ['1']);
  }
  Future<LoginCheck?> query(String id) async {
    Database db = await getDatabase();
    List<Map<String,dynamic>> maps =
    await db.query(name,where: "id = ?",whereArgs: [id]);
    if(maps.isNotEmpty){
      LoginCheck loginCheck = LoginCheck.fromMap(maps.first);
      return loginCheck;
    }
    return null;
  }
}
```



# 在页面初始化时使用异步

有时候需要在页面构建之前进行一次异步操作（是不是可以说我们无法真正的在构建之前进行异步操作），其实异步内的内容是到最后才去执行的。

## 踩坑一

![image-20220720203447362](C:\Users\asus\AppData\Roaming\Typora\typora-user-images\image-20220720203447362.png)

原因：在执行build()之前，super.initState()没有得到执行。

## 踩坑二

![image-20220720203511266](C:\Users\asus\AppData\Roaming\Typora\typora-user-images\image-20220720203511266.png)

结果：_load()没有在构建页面之前执行

原因：异步嘛~

顺便一提，看到网上还有人这么写

```dart
  _load() async {
    await ...
  }

  @override
  void initState (){
    super.initState();
    Future.delayed(Duration.zero, () => setState(() {
　　    _load();
}));
  }

```

嗯……是sleep()的味道呢

## 解决方案：

最终的解决方案嘛，也不能说很完美。大体思路就是，先设一个初始值，让页面第一次构建的时候使用这个值；等到拿到回调的结果后，再赋新值，同时setState()刷新。

此时，初始值很关键。大概就是第一次使用时连续闪出两个页面（第一个太快看不见，只能看到第二个），之后无影响和第一次无违和感，之后每次都闪一下的区别。

![image-20220720205144139](C:\Users\asus\AppData\Roaming\Typora\typora-user-images\image-20220720205144139.png)

# response转map

使用dio获得的response对象的data.toString()是这样的

{code: 0, message: success, data: [6afe0c0544625b5204b2588f9021adc5, 0b49ef4b6388c3303082800980b6bca4]}

也就是说，没有引号而且在奇怪的地方有空格，无法通过json转成map

需要经过下面的操作

```dart
s = s.replaceAll(" ", "");
s = s.replaceAll("{", "{\"");
s = s.replaceAll(":", "\":\"");
s = s.replaceAll(",", "\",\"");
s = s.replaceAll("}", "\"}");
s = s.replaceAll("\"[", "[\"");
s = s.replaceAll("]\"", "\"]");
```

# 画UI

## appbar的颜色

一般颜色可以改

```dart
ThemeData(
        primarySwatch: Colors.xxx,
      ),
```

但不能是Colors.white或Colors.black，因为它们不被认为是MaterialColor，此外也不能使用自定义的颜色，但是我们可以通过自定义一个产生MaterialColor的函数来自定义主题色

```dart
MaterialColor createMaterialColor(Color color) {
  List strengths = <double>[.05];
  Map<int, Color> swatch = {};
  final int r = color.red, g = color.green, b = color.blue;
  for (int i = 1; i < 10; i++) {
    strengths.add(0.1 * i);
  }
  strengths.forEach((strength) {
    final double ds = 0.5 - strength;
    swatch[(strength * 1000).round()] = Color.fromRGBO(
      r + ((ds < 0 ? r : (255 - r)) * ds).round(),
      g + ((ds < 0 ? g : (255 - g)) * ds).round(),
      b + ((ds < 0 ? b : (255 - b)) * ds).round(),
      1,
    );
  });
  return MaterialColor(color.value, swatch);
}
```

这样就可以写成下面这种形式

```dart
ThemeData(
        primarySwatch: createMaterialColor(Color(0x9c0c13)),//山大红
      ),
```

```javascript
{
    "code":"1",
    "msg":"操作成功",
    "data":
    	{
            "nickName":"null",
            "name":"lhs","isAdmin":"false","avatar":"null"},
  "token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI0IiwiZXhwIjoxNjU5NDk4MDc3LCJpbmZvIjp7InBhc3N3b3JkIjoicmV0dXJuMDsiLCJzaWQiOiIyMDIxMDAzMDAyMDkifX0.TRHfkKYd5couo6otPryU9WLOEo7aaQOIq5OuqANVEok"
}
```



StaggeredGridView弃用了
