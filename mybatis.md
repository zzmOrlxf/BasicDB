```xml
<![CDATA[
    Since this is a CDATA section
    
]]>
```

in Mybatis, when you use annotation ,and use the <script> ,the symbol like > or < or else would be recognize as in XML. so It would get error. 

simple use 

<![CDATA[  ...   ]]>

can easyly fix the problem.