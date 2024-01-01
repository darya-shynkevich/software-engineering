
![Pasted image 20230826020851](../../_Attachments/Pasted%20image%2020230826020851.png)

*==It is a distributed network of servers that caches content close to end users.==* CDNs are designed to quickly transfer assets needed for loading web content, such as HTML pages, JavaScript files, stylesheets, images, and videos. CDNs improve website performance and reduce latency by storing cached static content in multiple locations worldwide. When a user requests a webpage, the CDN server closest to the user’s geographical location serves up the content. CDNs can also enhance security and reliability by providing redundancy in case of outages or cyberattacks.

CDNs function by storing cached content in Points of Presence (PoPs), which are strategically placed servers around the world. These PoPs contain caching servers hosting identical copies of participating websites. When a user requests a website, the PoP nearest to them responds, delivering the cached content faster than if it were served by the original web server. CDNs can also communicate with the original server to deliver uncached or dynamically generated content.

CDNs don’t replace the need for web hosting but complement it by caching content at the network edge to improve website performance and reliability. Popular websites, including major platforms like Facebook, Netflix, and Amazon, utilize CDNs to enhance user experience and content delivery efficiency.

# References:

1. [Что такое CDN и как это работает?](https://habr.com/ru/companies/selectel/articles/463915/)
2. [System Design Basics: Navigating the World of CDN](https://levelup.gitconnected.com/system-design-basics-navigating-the-world-of-cdn-b59b92473593)
3. [Content delivery network](https://github.com/donnemartin/system-design-primer#content-delivery-network)
4. [A Beginner’s Guide to CDN: What it is and How it Works](https://medium.com/bytebytego-system-design-alliance/a-beginners-guide-to-cdn-what-it-is-and-how-it-works-f06946288fbb)
5. [Content Delivery Network (CDN)](https://medium.com/thedevproject/the-fundamental-knowledge-of-system-design-14-content-delivery-network-cdn-d5d16af9153)
6. [CDN](https://medium.com/@karan99/system-design-content-delivery-network-cdn-1c354e1e4834) A content delivery network (CDN) is a geographically distributed group of servers that work together to provide fast delivery of internet content. Generally, static files such as HTML/CSS/JS, photos, and videos are served from CDN.