On IIS, find your aspx page, right-click the file and select "Switch to Features View". On IIS group, double-click "HTTP Response Headers", remove/delete the item "X-UA-compatible | IE=EmulateIE7 | Inherited"
It took me 2 weeks to find this solution, since I have the latest plugin working perfectly with the Drag and Drop files on a html file and IE10 and whenever I was moving the code to an aspx file on my VS2010 project and running the solution, when I drag and drop files to IE10, it was rendering the files and opening the images on the browser, just like older version of IE... very irritating!
Now it works like a charm!
Thought I should share in case you have the same issue.

![IIS_fix](https://f.cloud.github.com/assets/2636051/520496/b2f3f1da-bf39-11e2-995f-82591e88b4b0.jpg)