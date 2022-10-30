# DocumentBuilderFactory解析XML

(1) javax.xml.parsers 包中的DocumentBuilderFactory用于创建DOM模式的解析器对象 ， DocumentBuilderFactory是一个抽象工厂类，它不能直接实例化，但该类提供了一个newInstance方法 ，这个方法会根据本地平台默认安装的解析器，自动创建一个工厂的对象并返回。

(2) 调用 DocumentBuilderFactory.newInstance() 方法得到创建 DOM 解析器的工厂。

```java
DocumentBuilderFactory doc=DocumentBuilderFactory.newInstance();
```


(3) 调用工厂对象的 newDocumentBuilder方法得到 DOM 解析器对象。

```java
DocumentBuilder db=doc.newDocumentBuilder();
```


(4) 把要解析的 XML 文档转化为输入流，以便 DOM 解析器解析它

```java
InputStream is= new  FileInputStream("test.xml");    

```


(5) 调用 DOM 解析器对象的 parse() 方法解析 XML 文档，得到代表整个文档的 Document 对象，进行可以利用DOM特性对整个XML文档进行操作了。

```java
 Document doc=dombuilder.parse(is);
```


(6) 得到 XML 文档的根节点

```
Element root=doc.getDocumentElement();
```


(7) 得到节点的子节点

```
  NodeList users=root.getChildNodes();
```

相关案例

```java
package cn.com.snxun.test;

import java.io.File;
import java.io.FileInputStream;
import java.io.InputStream;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;

import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;

public class XmlReader {
    public XmlReader() {
        DocumentBuilderFactory domfac = DocumentBuilderFactory.newInstance();
        try {
            DocumentBuilder domBuilder = domfac.newDocumentBuilder();
            InputStream is = new FileInputStream(new File(
                    "C:/Users/sony/Desktop/test.xml"));
            Document doc = domBuilder.parse(is);
            Element root = doc.getDocumentElement();
            NodeList users = root.getChildNodes();
            if (users != null) {
                for (int i = 0; i < users.getLength(); i++) {
                    Node user = users.item(i);
                    // TEXT_NODE 说明该节点是文本节点
                    // ELEMENT_NODE 说明该节点是个元素节点
                    if (user.getNodeType() == Node.ELEMENT_NODE) {
                        // （7）取得节点的属性值
                        // String email = user.getAttributes()
                        // .getNamedItem("email").getNodeValue();
                        // System.out.println(email);
                        // 注意，节点的属性也是它的子节点。它的节点类型也是Node.ELEMENT_NODE
                        // （8）轮循子节点
                        for (Node node = user.getFirstChild(); node != null; node = node
                                .getNextSibling()) {
                            if (node.getNodeType() == Node.ELEMENT_NODE) {
                                if (node.getNodeName().equals("name")) {
                                    String name = node.getNodeValue();
                                    String name1 = node.getFirstChild()
                                            .getNodeValue();
                                    System.out.println("name==" + name);
                                    System.out.println("name1==" + name1);
                                }
                                if (node.getNodeName().equals("price")) {
                                    String price = node.getFirstChild()
                                            .getNodeValue();
                                    System.out.println(price);
                                }
                            }
                        }
                    }
                }
            }
            NodeList node = root.getElementsByTagName("string");
            if (node != null) {
                for (int i = 0; i < node.getLength(); i++) {
                    Node str = node.item(i);
                    String s = str.getFirstChild().getNodeValue();
                    System.out.println(s);
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }

    }

    public static void main(String[] args) {
        XmlReader xmlReader = new XmlReader();

    }
}

```



```xml
<?xml version="1.0" encoding="GB2312" standalone="no"?>  
<users>
    <user email="www.baidu.com">
        <name>张三</name>
        <age>18</age>
        <sex>男</sex>    
    </user>
    <user>
        <name>李四</name>
        <age>16</age>
        <sex>女</sex>    
    </user>
    <user>
        <name>王五</name>
        <age>25</age>
        <sex>不明</sex>   
    </user>

</users>
```

