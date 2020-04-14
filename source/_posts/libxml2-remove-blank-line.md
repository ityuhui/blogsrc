title: libxml2 删除节点以后出现空行怎么办

date: 2019-08-14 17:05:03

categories:
- C语言

tags:
- c
- libxml2

---

## 问题的产生

使用libxml2操作XML的时候，有的时候会调用

```c
xmlUnlinkNode(node_to_del);
xmlFreeNode(node_to_del);
```

来删除节点，但是执行了之后，保存成XML文件的时候，会在删除的节点那一行显示出一个空行，很不美观。

<!--more-->

## 问题的原因

xmlNodePtr指向的元素，其实并不全是XML的元素节点（XML_ELEMENT_NODE），还会有一些用于缩进显示的节点（XML_TEXT_NODE），在删除元素节点的时候，需要把这个元素节点之前的文本节点也删除掉。

## 实例代码

```c
node_to_del = xml;
node_for_text_indent = xml->prev;
xml = xml->next;

xmlUnlinkNode(node_to_del);
xmlFreeNode(node_to_del);

xmlUnlinkNode(node_for_text_indent); // delete the useless TEXT indent node
xmlFreeNode(node_for_text_indent);
```

## 注意事项

要删除元素节点前面（->prev）的文本节点，不要删除元素节点后面（->next）的文本节点。
