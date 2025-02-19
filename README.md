# TipsForCodeing

How to edit Readme
https://docs.github.com/ja/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax


## C#

### DeepCopy

**Useing BinaRyFormatter**

https://qiita.com/gazf/items/5d5393ac37c2304ecf0a

```
public static T BinaryFomartterClone<T>(T src)
{
    using (var ms = new MemoryStream())
    {
        var binaryFormatter = new System.Runtime.Serialization
            .Formatters.Binary.BinaryFormatter();
        binaryFormatter.Serialize(ms, src);
        ms.Seek(0, SeekOrigin.Begin);
        return (T)binaryFormatter.Deserialize(ms);
    }
}
```
