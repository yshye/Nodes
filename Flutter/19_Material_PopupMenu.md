
## 介绍
> 弹出式菜单


### 示例
```dart
import 'package:flutter/material.dart';

class PopupMenuMaterial extends StatefulWidget {
  @override
  State<StatefulWidget> createState() {
    return _PopupMenuDemo();
  }
}

class _PopupMenuDemo extends State<PopupMenuMaterial> {
  String _normalTitle = "NormalPopMenu";
  String _iconTitle = "IconPopMenu";
  String _checkedTitle = "CheckedPopupMenuItem";

  @override
  Widget build(BuildContext context) {
    return _popupMenuList(context);
  }

  ///
  /// 创建
  ///
  Widget _popupMenuList(BuildContext context) {
    return ListView(
      padding: EdgeInsets.all(10.0),
      children: <Widget>[
        _normalPopMenu(),
        Divider(),
        _iconPopMenu(),
        Divider(),
        _checkedPopupMenuItem(),
        Divider(),
        _popupMenuDivider(),
        Divider(),
        _showMenu(context),
        Divider(),
      ],
    );
  }

  ///
  /// 1. 默认PopMenu
  ///
  Widget _normalPopMenu() {
    return PopupMenuButton<String>(
      child: Text(
        _normalTitle,
      ),
      itemBuilder: (BuildContext context) => <PopupMenuItem<String>>[
            new PopupMenuItem<String>(
                value: 'Item One', child: new Text('Item One')),
            new PopupMenuItem<String>(
                value: 'Item Two', child: new Text('Item Two')),
            new PopupMenuItem<String>(
                value: 'Item Three', child: new Text('Item Three')),
            new PopupMenuItem<String>(
                value: 'I am Item Four', child: new Text('I am Item Four'))
          ],
      onSelected: (String value) {
        setState(() {
          if (value != null) {
            _normalTitle = "NormalPopMenu : " + value;
          }
        });
      },
    );
  }

  ///
  /// 2. 带有Icon
  ///
  Widget _iconPopMenu() {
    return PopupMenuButton<String>(
      child: Text(
        _iconTitle,
      ),
      itemBuilder: (BuildContext context) => <PopupMenuItem<String>>[
            PopupMenuItem<String>(
              value: 'Icon One',
              child: ListTile(
                leading: Icon(Icons.looks_one),
                title: Text("Item one"),
              ),
            ),
            PopupMenuItem<String>(
              value: 'Icon Two',
              // 若需要处理带图标的样式时，官网提供的 Demo 是借助的 ListTile 来处理的，
              // 但是小菜测试发现图标与文字距离偏大，原因在于 ListTile 默认左侧图标 leading 距离不可直接调整，
              // 建议用 Row 或其他方式调整
              child: Row(
                children: <Widget>[
                  Padding(
                      padding: EdgeInsets.fromLTRB(0.0, 0.0, 8.0, 0.0),
                      child: Icon(Icons.looks_two)),
                  Text('Item Two')
                ],
              ),
            ),
            PopupMenuItem<String>(
              value: 'Icon Three',
              child: ListTile(
                leading: Icon(Icons.threed_rotation),
                title: Text("Item Three"),
              ),
            ),
            PopupMenuItem<String>(
              value: 'Icon Four , No Icon !',
              child: ListTile(
                title: Text("Item Four,No Icon"),
              ),
            )
          ],
      onSelected: (String value) {
        setState(() {
          _iconTitle = "IconPopMenu : " + value;
        });
      },
    );
  }

  /// 3. CheckedPopupMenuItem
  Widget _checkedPopupMenuItem() {
    return PopupMenuButton<String>(
      child: Text(_checkedTitle),
      itemBuilder: (BuildContext context) => <PopupMenuItem<String>>[
            CheckedPopupMenuItem<String>(
              checked: _checkedTitle == 'Checked One',
              value: 'Checked One',
              child: Text("Check One"),
            ),
            CheckedPopupMenuItem<String>(
              checked: _checkedTitle == 'Checked Two',
              value: 'Checked Two',
              child: Text("Check Two"),
            ),
            CheckedPopupMenuItem<String>(
              checked: _checkedTitle == 'Checked Three',
              value: 'Checked Three',
              child: Text("Check Three"),
            ),
            CheckedPopupMenuItem<String>(
              checked: _checkedTitle == 'Checked Four',
              value: 'Checked Four',
              child: Text("Check Four"),
            ),
          ],
      onSelected: (String value) {
        setState(() {
          _checkedTitle = value;
        });
      },
    );
  }

  /// 4. PopupMenuDivider
  ///  PopupMenuDivider 是一条水平分割线，注意数组要使用父类 PopupMenuEntry，配合其他 item 样式共同使用。
  ///  PopupMenuDivider 可以调整高度，但无法调整颜色，有需要的话可以进行自定义。
  Widget _popupMenuDivider() {
    return PopupMenuButton<String>(
      itemBuilder: (BuildContext context) => <PopupMenuEntry<String>>[
            PopupMenuItem<String>(
              value: 'Item One',
              child: Text('Item One'),
            ),
            PopupMenuDivider(
              height: 1.0,
            ),
            PopupMenuItem<String>(
              value: 'Item Two',
              child: Text('Item Two'),
            ),
            PopupMenuDivider(
              height: 4.0,
            ),
            PopupMenuItem<String>(
              value: 'Item Three',
              child: Text('Item Three'),
            ),
            PopupMenuDivider(
              height: 2.0,
            ),
            PopupMenuItem<String>(
              value: 'Item Four',
              child: Text('Item Four'),
            ),
          ],
      child: Text("PopupMenuDivider"),
    );
  }

  /// 5. showMenu
  Widget _showMenu(BuildContext context) {
    return Text("// TODO showMenu");
  }
}

```