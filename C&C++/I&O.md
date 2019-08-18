### I/O

#### 调用python

```c++
    //初始化python
    Py_Initialize（）；
    if（！Py_IsInitialized（）)
    {
        return;
    }
    
    PyRun_SimpleString(“import sys”);
    PyRun_SimpleString(“sys.path.append(‘.’)”);
    
    PyObject* pModule = PyImport_ImportModule(“模块名称”);
    if(!pModule)
    {
        return;
    }
    
    PyObject* pFunc = PyObject_GetAttrString(pModule,”方法名称”);
    if(!pFunc)
    {
        return;
    }
    
    /*  s 表示字符串
        i 表示整型变量
        f 表示浮点变量
        o 表示python对象    */
    PyObject* pArgs = PyTuple_New(2);
    PyTuple_SetItem(pArgs, 0, Py_BuildValue(“s”, “参数1”.toStdString().c_str()));
    PyTuple_SetItem(pArgs, 1, Py_BuildValue(“s”, “参数2”.toStdString().c_str()));
    
    PyObject* object = PyObject_Callobject(pFunc, pArgs);
    int result = -1;
    PyArg_Parse(object, “i”, &result);          //转化调用结果
    
    //结束，释放python
    Py_Finalize();
```

#### ini文件

```c++
    QSettings configIni(“文件名.ini”, QSettings::IniFrmat);
    //读
    QString value = configIni.value(“组名/属性名”).toString();
    //写
    configIni.beginGroup(“组名”);
    configIni.setValue(“属性名”, 属性值);
```

#### xml文件

```c++
#include <QtXml>
#include <QDomDocument>
```

##### 解析xml

```c++
    QFile xmlFile(“文件名”);
    if(!xmlFile.exists())
    {
   	    return false;
    }
    if(!xmlFile.open(QFile::ReadOnly))
    {
   	    return false;
    }
        
    QDomDocument doc;
    if(!doc.setContent(&xmlFile))
    {
        return false;
    }
    QDomElement root = doc.documentElement();
    if(root.tagName() != “一级节点名称”)
    {
        return false;
    }
    QDomNode node = root.firstChild();
    if(node.isNull)
    {
        return false;
    }
    
    QDomNode childNode = node.firstChild();     //访问子节点
    QDomNode siblingNode = node.nextSibling();  //访问同级节点
    
    QString tagName = node.toElement().tagName();       //当前节点名称
    QString text = node.toElement().text();             //当前节点文本内容
    QString attributeValue = node.toElement().attribute(“属性名”);  //当前节点属性值
    
    xmlFile.close();
```

##### 生成xml

```c++
    QDomDocument doc;
    
    //设置前缀
    QString strHeader(”version=\”1.0\” encoding=\”UTF-8\””);
    doc.appendChild(doc.createProcessingInstruction(“xml”, strHeader));
    
    QDomElement element = doc.createElement(“节点名称”);        //创建的节点名称
    element.setAttribbute(“属性名”, 属性值(字符串));            //设置节点属性
    
    QDomText text = doc.createTextNode(“文本内容”);             //设置文本内容
    element.appendChild(text);
    
    doc.appendChild(element);                                   //连接子节点
    
    QFile file(“文件名”);
    if(!file.open(QIODevice::WriteOnly))
    {
        return false;
    }
    QTextstream stream(&file);
    doc.save(stream, 4);
    
    file.close();
```

#### dll文件

```c++
#include <windows.h>
```

##### 生成dll

```c++
#ifndef 文件名_EXPORTS
#define 文件名_EXPORTS __declspec(dllexport)
#endif

#ifdef __cplusplus
extern “C” {
#endif
...
#ifdef __cplusplus
}
#endif
```

##### 动态调用

```c++
typedef 返回类型 (*PtrFunc)(参数);

    HMODULE hDll = LoadLibrary(L”文件名.dll”);
    if(hDll != NULL)
    {
        PtrFunc pFunc = (PtrFunc)GetProcAddress(hDll, “方法名”);
        if(pFunc != NULL)
        {
            pFunc(参数);
        }
    }
```

