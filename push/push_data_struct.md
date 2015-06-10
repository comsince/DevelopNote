## push数据格式设计

* 1.打开应用主界面数据格式
```
{
    "taskid": "taskid",
    "package_name": "your packagename",
    "push_type": "0",
    "notify_type": "1",
    "content": "content",
    "title": "title",
    "notify_effect": "notify_launcher_activity",
    "extra": {
     
    }
}
```

* 2.打开应用任意activity数据格式
```
{
    "taskid": "taskid",
    "package_name": "your packagename",
    "push_type": "0",
    "notify_type": "1",
    "content": "content",
    "title": "title",
    "notify_effect": "notify_activity",
    "extra": {
        "intent_activity_name": "testActivity"
    }
}
```

* 3.打开 网页面的数据格式
```
{
    "taskid": "taskid",
    "package_name": "your packagename",
    "push_type": "0",
    "notify_type": "1",
    "content": "content",
    "title": "title",
    "notify_effect": "notify_web",
    "extra": {
        "web_uri": "https://www.baidu.com/"
    }
}
```
* 4.打开带参数的应用页面的数据格式
```
{
    "taskid": "taskid",
    "package_name": "your packagename",
    "push_type": "0",
    "notify_type": "1",
    "content": "content",
    "title": "title",
    "notify_effect": "notify_activity",
    "extra": {
        "key": "value",
        "intent_activity_name": "testActivity"
    }
}
```
