# JSR-330
Java Specification Requests Java规范提案
# set
元素不可重复。
# list
元素有放入顺序，元素可重复。  
ArrayList：基于动态数组实现，支持随机访问。  
Vector：和 ArrayList 类似，但它是线程安全的。   
LinkedList：基于双向链表实现，只能顺序访问，但是可以快速地在链表中间插入和删除元素。不仅如此，LinkedList 还可以用作栈、队列和双向队列。  
# HashMap
HashMap 底层是hash数组和单向链表实现，数组中的每个元素都是链表(Node<K,V>[] table) 。当链表长度超过8时，链表转为红黑树。  
Hashmap遍历
```
for (Map.Entry entry: map.entrySet()) {
    System.out.println("key:" + entry.getKey() +", value:" +  entry.getValue());
}

for (String key: map.keySet()) {
    System.out.println("key:" + key + ", value:" + map.get(key));
}

Iterator<Map.Entry<String, String >> entries = map.entrySet().iterator();
while (entries.hasNext()) {
    Map.Entry<String, String> entry = entries.next();
    System.out.println("key:" + entry.getKey() +", value:" +  entry.getValue());
}
```
# String StringBuffer StringBuild

# io

