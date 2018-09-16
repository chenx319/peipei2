##  背景
---
给实体(entity)打一个标记(flag)  
entity: 可以是node ,也可以是user  
flag 也就是标记,我们有两种类型的标记, 

flag 类型 | 用途 | 说明
---- | ---|---
following | 关注| 用于一个用户"关注"另外一个用户
like |  喜欢/点赞 | 用户一个用户"喜欢"一个内容(node)
collect| 收藏| 用户"收藏"一个内容(node)

---



## REST API 如下

```
curl -X POST -H 'Content-Type: application/json' -i 'http://www.uaes.site:8088/d86/api/v1/flagrest' --data '{
  "entity_id":59,
  "entity_type":"node",
  "flag_id": "like", 
  "my_uid": 1,
  "flag_action": "flag"
}'
```

  XX |  YY  | 说明
---- | ---|---
method| POST|
URL | http://www.uaes.site:8088/d86/api/v1/flagrest| 
header | ontent-Type: application/json | 
body|  "entity_id":59,| entity的id, </br> 如果标记的是node,则是nid,</br>如果标记的是user,则是user id
 xx| "entity_type":"node",| entity 类型,可以是"node"或者"user"
 xx| "flag_id": "like", | 标记类型, </br>用户关注使用 following, </br>内容"点赞"使用 like </br>内容"收藏"用 collect
 xx| "my_uid": 1,| 用户id
xx|"flag_action": "flag"|动作,flag (标记) unflag (取消标记))


---


# 由此的引起的改动
获取 用户视频的 REST API 升级到v3  
```
curl -X GET -i 'http://www.uaes.site:8088/d86/api/v3/uservideo?_format=json&page=1'
```

增加了如下两个信息
```
        "Flags_like":"No",
        "Flags_collect":"No"
```        
> Flags_like = "No",  表示当前用户没有"点赞"该视频  
> Flags_like = "Yes", 表示当前用户已经"点赞"该视频

完整的response 如下
``` json
[
    {
        "title":"<a href="/d86/node/58" hreflang="en">sample video test</a>",
        "field_media_video_file":"
<span class="file file--mime-video-mp4 file--video"> <a href="http://www.uaes.site:8088/d86/sites/default/files/2018-09/1532848781578_2.mp4" type="video/mp4; length=1290662">1532848781578.mp4</a></span>
",
        "field_media_oembed_video":"",
        "comment_count":"0",
        "field_user_statement":"",
        "field_user_nickname":"购肿白蓟",
        "user_picture":"",
        "like_count":"0",
        "collect_count":"0",
        "body":"",
        "nid":"58",
        "Flags_like":"No",
        "Flags_collect":"No"
    },
    {
        "title":"<a href="/d86/node/57" hreflang="en">sample video test</a>",
        "field_media_video_file":"
<span class="file file--mime-video-mp4 file--video"> <a href="http://www.uaes.site:8088/d86/sites/default/files/2018-09/1532848781578_1.mp4" type="video/mp4; length=1290662">1532848781578.mp4</a></span>
",
        "field_media_oembed_video":"",
        "comment_count":"0",
        "field_user_statement":"",
        "field_user_nickname":"购肿白蓟",
        "user_picture":"",
        "like_count":"0",
        "collect_count":"0",
        "body":"",
        "nid":"57",
        "Flags_like":"No",
        "Flags_collect":"No"
    },
    {
        "title":"<a href="/d86/node/54" hreflang="en">sample video test</a>",
        "field_media_video_file":"
<span class="file file--mime-video-mp4 file--video"> <a href="http://www.uaes.site:8088/d86/sites/default/files/2018-09/1532848781578_0.mp4" type="video/mp4; length=1290662">1532848781578.mp4</a></span>
",
        "field_media_oembed_video":"",
        "comment_count":"0",
        "field_user_statement":"",
        "field_user_nickname":"购肿白蓟",
        "user_picture":"",
        "like_count":"0",
        "collect_count":"0",
        "body":"",
        "nid":"54",
        "Flags_like":"No",
        "Flags_collect":"No"
    }
]
```