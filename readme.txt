Serialize功能#
Unity3D 中可以帮助用户将 成员变量 在Inspector中显示，并且定义Serialize关系。
所有显示在Inspector 中的属性都同时具有Serialize功能，这些值就会被保存成二进制文件。


1. public 变量#
在没有加入任何Attribute的前提下，public变量是默认被视为可以被Serialize的。

2. [SerializeField] Attribute #
强制unity去序列化一个私有域

这是一个内部的unity序列化功能，有时候我们需要Serialize一个private或者protected的属性，这个时候可以使用[SerializeField]这个Attribute:

[SerializeField]
protected int  protectedParam = 0;
注意: 这样定义出的成员变量也是会在Inspector中显示出来。

在Unity最新的UI系统中，UI属性上方全部添加[SerializeField] ，如下所示

[SerializeField]
private string str1;

3. 单独的class或struct #
Serializable是.Net自带的序列化

有时候我们会自定义一些单独的class/struct, 由于这些类并没有从 MonoBehavior 派生所以默认并不被Unity3D识别为可以Serialize的结构。自然也就不会在Inspector中显示。我们可以通过添加 [System.Serializable]这个Attribute使Unity3D检测并注册这些类为可Serialize的类型。具体做法如下：

[System.Serializable]
public class objItem{
    public int itemparam1= 5;
    public int itemparam1=2= 10;
}
注意：Serializable只可以对class,struct,enum,delegate进行序列化，不可以对属性序列化

4. ScriptableObject#
ScriptableObject 类型经常用于存储一些unity3d本身不可以打包的一些object，比如字符串，一些类对象等。用这个类型的子类型，则可以用BuildPipeline打包成assetbundle包供后续使用，非常方便，具体请参考 [cb]ScriptableObject 序列化

NonSerialize的变量的定义方法#
4.1. protected, private, internal 变量#
默认情况下，protected, private, internal变量将不会被serialize.

4.2. [System.NonSerialized] Attribute#
有时候我们需要定义一些public变量方便操作，但是又不希望这些变量保留。这个时候可以利用[System.NonSerialized]来完成这个操作:

[System.NonSerialized] public float noSerializeParam= 1.0f;
4.3. readonly, const, static 修饰符#
如果变量加入了readonly, const, static等修饰符，无论他的serialize设置如何，都将不会进行serialize

4.4. Dictionary的序列化实现
复制代码
在Inspector中的显示#
变量在Inspector中会根据变量的大写字母来隔开来显示