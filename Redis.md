注：先在“依赖项（引用）”添加NuGet包——搜索“servicesta”

![514161242514](https://s2.loli.net/2022/05/14/eZWt6kbBm7KTUao.png)

视频     [C#.NET Core + Redis](https://www.bilibili.com/video/BV1mY4y1i7R3?p=3)

# Redis数据类型

|              |                |
| ------------ | -------------- |
| String       | 字符串         |
| Hash         | 哈希           |
| List         | 集合           |
| Set          | 去重集合       |
| Zset         | 有序的去重集合 |
| BitMaps      |                |
| HyperLogLoss |                |
| Streams      |                |

```csharp
// id地址   端口号
using(IRedisClient client = new RedisClient("192.168.3.210",6357)){
	// 删除当前数据库中的所有key
    client.FlushDb();
    
    // 新增key，默认的是做了序列化存储
    client.Set<string>("name","clay");
    
    // 读取  
    var values = client.Get<string>("name");
    Console.WriteLine(values);
    
	// 批量写
    var dics = new  Dictionary<string,string>();
    dics.Add("id","001");
    dice.Add("name","张三");
    dice.Add("address","天津");
    client.SetAll(dics);	// 批量写入
    
    //  批量读取
    var list = client.GetAll<string>(new string[]{"id","name","address"});
    foreach(var item in list){
        Console.WriteLine(item);
    }
    
}
```

