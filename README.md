# The name of an affected Product.
SpringbootCMS
# The affected or fixed version(s).
SpringbootCMS 1.0
# The CVE ID for the entry
CVE-2023-43191
# A prose description
SpringbootCMS foreground message can be embedded malicious code saved in the database. When users browse the comments, these malicious codes embedded in the HTML will be executed, and the user's browser will be controlled by the attacker, so as to achieve the special purpose of the attacker, such as cookie theft
# Other supplement
Hole address: http://ip:8888/guestbook
The code download address: [https://gitee.com/heyewei/SpringBootCMS.git](https://gitee.com/heyewei/SpringBootCMS.git)
Vulnerability location: After logging in to the system, the custom data of the expanded content, the location of the customer name at the time of creation has sql time blind annotation.
# Exploit

![image.png](https://cdn.nlark.com/yuque/0/2023/png/21570886/1694499385096-4a71947c-5646-4591-8662-41bc6675f8eb.png#averageHue=%23ece1b8&clientId=ua4912bdd-475e-4&from=paste&height=840&id=ub9ed8555&originHeight=963&originWidth=1760&originalType=binary&ratio=1.1458333730697632&rotation=0&showTitle=false&size=83457&status=done&style=none&taskId=u32ef587e-5cc2-445d-9bec-5cf3edd867f&title=&width=1535.9999467329565)
Enter malicious content after submission
![image.png](https://cdn.nlark.com/yuque/0/2023/png/21570886/1694500099508-a44abef1-e80a-492e-a8dd-ac35cff69444.png#averageHue=%23fefefe&clientId=ua4912bdd-475e-4&from=paste&height=828&id=u43f29763&originHeight=949&originWidth=1518&originalType=binary&ratio=1.1458333730697632&rotation=0&showTitle=false&size=61922&status=done&style=none&taskId=u1f5e5806-c1fa-4b72-8a15-f7912d0e6c0&title=&width=1324.799954057175)
Requires background user review before the foreground can see the content (execute malicious content)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/21570886/1694500198353-e870b104-530c-417e-84c4-f233dedc66f2.png#averageHue=%238d8d90&clientId=ua4912bdd-475e-4&from=paste&height=752&id=u62564cba&originHeight=862&originWidth=1321&originalType=binary&ratio=1.1458333730697632&rotation=0&showTitle=false&size=16618&status=done&style=none&taskId=udb3856eb-7553-40f2-8289-10a58c12d65&title=&width=1152.8726872921793)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/21570886/1694500211281-45d93a26-87cb-45f0-b718-ea3a761ee81d.png#averageHue=%2376767e&clientId=ua4912bdd-475e-4&from=paste&height=844&id=u569b11d4&originHeight=967&originWidth=1895&originalType=binary&ratio=1.1458333730697632&rotation=0&showTitle=false&size=53737&status=done&style=none&taskId=ua2761ade-7fee-4008-9f1f-be5085c32f7&title=&width=1653.8181244653138)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/21570886/1694500221432-ff3e57a4-4bc2-4c40-ac4b-5815dfbfa77e.png#averageHue=%2375767e&clientId=ua4912bdd-475e-4&from=paste&height=851&id=u060633fd&originHeight=975&originWidth=1898&originalType=binary&ratio=1.1458333730697632&rotation=0&showTitle=false&size=54430&status=done&style=none&taskId=u2c299e34-be33-43a8-b5b2-02ceb513c72&title=&width=1656.4363061926995)
After the background check, malicious xss is also executed in the foreground.
http://ip:port/guestbook?pageNumber=0
![image.png](https://cdn.nlark.com/yuque/0/2023/png/21570886/1694500309019-0a88073a-ade7-4cfc-833b-9ea29ee9b02c.png#averageHue=%23fefefe&clientId=ua4912bdd-475e-4&from=paste&height=768&id=u812314f7&originHeight=880&originWidth=1607&originalType=binary&ratio=1.1458333730697632&rotation=0&showTitle=false&size=19550&status=done&style=none&taskId=ua82b742b-babd-48d3-ae2c-d5d3db62d30&title=&width=1402.4726786362846)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/21570886/1694500345693-fe981bc2-da38-4f21-a67f-18e090d14d49.png#averageHue=%23fefefe&clientId=ua4912bdd-475e-4&from=paste&height=841&id=u3c6780d4&originHeight=964&originWidth=1561&originalType=binary&ratio=1.1458333730697632&rotation=0&showTitle=false&size=19209&status=done&style=none&taskId=u59c325ba-3cdf-49bb-9f28-276454bf2c9&title=&width=1362.327225483037)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/21570886/1694500360720-efb04306-594d-4a57-9977-69a8e96cf5ac.png#averageHue=%23d6bf95&clientId=ua4912bdd-475e-4&from=paste&height=860&id=u11d3c8fa&originHeight=985&originWidth=1664&originalType=binary&ratio=1.1458333730697632&rotation=0&showTitle=false&size=166465&status=done&style=none&taskId=ufae45125-95b2-48b3-954f-4c1e3afa0e6&title=&width=1452.2181314566133)

# Repair suggestion
To avoid storage XSS attacks, you are advised to defend against them in the following ways:
1. Perform reasonable verification of untrustworthy data obtained from databases or other back-end data stores (such as age can only be a number), filter special characters (such as' <, >, ', "' and '<script>, javascript', etc.).
2. Output code all untrusted data appropriately, depending on where the data will be placed in the HTML context (HTML tags, HTML attributes, JavaScript scripts, CSS, urls).
# Website source code
The front-end file is   src/main/webapp/templates/default/guestbook.html
![image.png](https://cdn.nlark.com/yuque/0/2023/png/21570886/1694501755736-d3a6df3f-8f2c-4941-9e02-a66d38c8eaf5.png#averageHue=%232d2c2b&clientId=ua4912bdd-475e-4&from=paste&height=688&id=pY8KL&originHeight=788&originWidth=1205&originalType=binary&ratio=1.1458333730697632&rotation=0&showTitle=false&size=102067&status=done&style=none&taskId=u623d7c15-069e-4a75-949f-5c0f26328c8&title=&width=1051.636327166598)

Message leaving interface /guestbook/save, file path:
src/main/java/com/cms/controller/front/GuestbookController.java From line 35
![image.png](https://cdn.nlark.com/yuque/0/2023/png/21570886/1694501707445-5f53596c-8052-4d37-83bb-b791abf98370.png#averageHue=%232f2d2b&clientId=ua4912bdd-475e-4&from=paste&height=550&id=uab7787a8&originHeight=630&originWidth=683&originalType=binary&ratio=1.1458333730697632&rotation=0&showTitle=false&size=90118&status=done&style=none&taskId=udebdb4f1-bf3c-44ae-85eb-d36382258b5&title=&width=596.0727066014825)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/21570886/1694502109372-451ff9be-0553-49a6-a275-b5e047e9a17e.png#averageHue=%237a6145&clientId=ua4912bdd-475e-4&from=paste&height=370&id=u02ad5410&originHeight=424&originWidth=878&originalType=binary&ratio=1.1458333730697632&rotation=0&showTitle=false&size=45534&status=done&style=none&taskId=u72a1b1d1-faad-43f8-abaa-8a04ea281d7&title=&width=766.2545188815544)
The code to save the message is executed, and there is no sitting filter operation. 
It is saved to the table named cms_guestbook.
![image.png](https://cdn.nlark.com/yuque/0/2023/png/21570886/1694502023233-e9dd8fa4-325f-46bb-b1b3-e2412400a32f.png#averageHue=%232e2e2c&clientId=ua4912bdd-475e-4&from=paste&height=443&id=ua243f66a&originHeight=508&originWidth=542&originalType=binary&ratio=1.1458333730697632&rotation=0&showTitle=false&size=39564&status=done&style=none&taskId=u9bfbafc7-03e2-4b5c-a283-4e510c47967&title=&width=473.01816541435363)
No escape occurred in the database.
![image.png](https://cdn.nlark.com/yuque/0/2023/png/21570886/1694502172086-015c1853-2295-4f5a-a35e-a9c8b4b4bf68.png#averageHue=%23f8f6f5&clientId=ua4912bdd-475e-4&from=paste&height=328&id=uf2c42680&originHeight=376&originWidth=1271&originalType=binary&ratio=1.1458333730697632&rotation=0&showTitle=false&size=32969&status=done&style=none&taskId=u9be3cff7-d4c7-4690-9af6-8527e7f94c3&title=&width=1109.2363251690838)
payload：\"><sCrIpT>alert("cve2")</sCrIpT>
